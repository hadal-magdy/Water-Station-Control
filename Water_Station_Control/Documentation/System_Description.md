# System Description - Water Station Control
## وصف النظام الكامل

---

## 1. نظرة عامة على النظام

المحطة بتغذي مدينة سكنية من خلال 5 طلمبات على درايفات Schneider ATV312. التحكم بيتم عن طريق S7-1214C مع Modbus RTU للتواصل مع الدرايفات.

---

## 2. منطق التشغيل

### 2.1 وضع Manual
- كل طلمبة بتشتغل مستقلة من زرار Start/Stop
- السرعة ثابتة (Default 50Hz = 100%)
- الـ Overload والـ Fault من الـ Drive بيوقفوا الطلمبة
- عداد الساعات بيكمل مع الـ Manual

### 2.2 وضع Automatic
```
قراءة الضغط من Transmitter (4-20mA)
        ↓
تحويل إلى بار (0-10)
        ↓
تحديد عدد الطلمبات المطلوبة:
  > 7.5 bar → 1 طلمبة
  5-7.5 bar → 2 طلمبة
  2.5-5 bar → 3 طلمبات
  < 2.5 bar → 4 طلمبات
        ↓
FB_PumpRotation بيحدد مين يشتغل حسب الـ Queue
        ↓
PID Controller بيضبط السرعة للوصول لـ 8.0 bar
        ↓
Ramp: السرعة بتزيد تدريجي (2% لكل 100ms)
```

### 2.3 منطق الـ PID

الـ PID بيشتغل على السرعة فقط (مش على عدد الطلمبات):
- عدد الطلمبات بيتحدد من zones الضغط
- السرعة بتتحكم في الضغط الدقيق داخل الـ Zone

```
Error = Target(8.0) - Pressure_Actual
Speed = 50% + Kp × Error + Ki × ∫Error
```

---

## 3. منطق الـ Rotation

### Queue System
```
Initial State:
Queue[1]=P1, Queue[2]=P2, Queue[3]=P3, Queue[4]=P4, Standby=P5

بعد 72 ساعة (مثلاً P1 أكثر شغل):
Queue[1]=P5, Queue[2]=P2, Queue[3]=P3, Queue[4]=P4, Standby=P1

العداد للـ rHoursSinceRotation بيترجع لصفر بعد الـ Rotation
```

### Fault Replacement
```
P1 عملت Fault وهي شغالة:
→ P1 تتوقف فوراً
→ Standby (P5) تدخل مكانها
→ P1 تبقى Standby الجديدة (لما تتصلح)
→ الـ Queue بيتعدل تلقائي
```

---

## 4. Communication Architecture

```
S7-1214C
    │
    └── CM 1241 RS485
            │
            ├── ATV312 #1 (Addr:1) → Pump 1
            ├── ATV312 #2 (Addr:2) → Pump 2
            ├── ATV312 #3 (Addr:3) → Pump 3
            ├── ATV312 #4 (Addr:4) → Pump 4
            └── ATV312 #5 (Addr:5) → Pump 5

بروتوكول: Modbus RTU
Baud: 19200, Even Parity, 1 Stop Bit
Cycle: كل طلمبة بتتسأل كل ~600ms (100ms × 3 steps × 2)
```

### Modbus Timing
```
State Machine: Write CMD → Write Speed → Read Status → Next Pump
OB35 (100ms) = كل scan بتتنفذ خطوة واحدة فقط
Full Cycle per pump = 300ms
All 5 pumps = 1.5 seconds
```

---

## 5. Safety Features

| Feature | التفاصيل |
|---------|---------|
| Overload Protection | NC contact → فولت فوري |
| Drive Fault Detection | Status Word bit 3 |
| Comm Error Detection | MB_ERROR + Retry 3 times |
| Auto Fault Replacement | Standby تدخل خلال < 2 scan cycles |
| Emergency Stop | Hardware wired to all drives enable |
| High Pressure Alarm | > 9.5 bar |
| Low Pressure Alarm | < 0.5 bar في Auto |

---

## 6. هيكل الـ Function Blocks

```
OB1 (Main)
├── FB_PressureControl    → يحسب عدد الطلمبات والسرعة
├── FB_PumpRotation       → يدير الـ Queue والـ Standby
├── FB_PumpControl #1-5   → يتحكم في كل طلمبة
└── (IO Read/Write)

OB35 (100ms Cyclic)
└── FC_ModbusComm         → تواصل Modbus مع الدرايفات

Data:
├── DB_SystemConfig       → إعدادات وحالة النظام
└── DB_PumpData           → بيانات الطلمبات الخمسة
```

---

## 7. ملاحظات لـ Commissioning

### ترتيب التشغيل الأول:
1. تأكد من عدم وجود حمولة (افصل المحركات للتجربة)
2. ضع كل الـ Selectors على Manual
3. اختبر كل زرار Start/Stop
4. اختبر كل Overload يدوياً
5. اختبر الـ Fault Lamps
6. شغل الطلمبات يدوياً وتأكد من الـ Modbus communication
7. اختبر قراءة الضغط (Transmitter calibration)
8. روح على Auto ببطء

### Tuning الـ PID:
- ابدأ بـ Kp=5, Ki=0.5
- لو الضغط بيتذبذب: خفض Kp
- لو بطيء في الوصول: زود Kp
- لو Steady State Error: زود Ki
