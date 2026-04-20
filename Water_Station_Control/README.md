# محطة مياه - Water Station Control System
## S7-1200 | ATV312 Modbus | WinCC SCADA

---

## هيكل المشروع

```
Water_Station_Control/
├── TIA_Portal/
│   ├── Source_Files/          ← ملفات SCL جاهزة للـ Import
│   │   ├── DB_SystemConfig.scl
│   │   ├── DB_PumpData.scl
│   │   ├── FB_PumpControl.scl
│   │   ├── FB_PressureControl.scl
│   │   ├── FB_PumpRotation.scl
│   │   ├── FC_ModbusComm.scl
│   │   └── OB1_Main.scl
│   └── IO_Config/
│       └── IO_Mapping.md
├── WinCC/
│   ├── Tag_List.md
│   ├── Screen_Design.md
│   └── Alarm_Config.md
├── EPLAN/
│   └── Panel_Layout.md
├── Dashboard/
│   └── index.html             ← Dashboard تفاعلي
└── Documentation/
    └── System_Description.md
```

---

## Hardware المستخدم

| المكون | الموديل |
|--------|---------|
| PLC CPU | S7-1214C DC/DC/DC |
| Communication Module | CM 1241 RS485 (للـ Modbus) |
| Analog Input | SM 1231 AI 4×13bit |
| Digital Input | SM 1221 DI 16×24VDC |
| Digital Output | SM 1222 DO 16×24VDC/0.5A |
| Drives | Schneider ATV312 × 5 |
| HMI | WinCC Runtime (PC) |

---

## طريقة تشغيل المشروع في TIA Portal

### الخطوات:

**1. فتح TIA Portal وإنشاء مشروع جديد**
- افتح TIA Portal V17 أو أحدث
- `File → New Project` → اسم المشروع: `Water_Station`
- اختار CPU: **6ES7 214-1AG40-0XB0** (S7-1214C)

**2. إضافة Hardware Modules**
```
في Devices & Networks:
- أضف CM 1241 RS485 في Slot 101
- أضف SM 1231 AI في Slot 2
- أضف SM 1221 DI في Slot 3
- أضف SM 1222 DO في Slot 4
```

**3. Import ملفات SCL**
```
في Project Tree:
- External Source Files → Add external file
- اختار كل ملفات .scl من مجلد Source_Files
- بعد الـ import: كليك يمين → Generate blocks from source
```
> ⚠️ **مهم:** Import بالترتيب ده:
> DB_SystemConfig → DB_PumpData → FB_PumpControl → 
> FB_PressureControl → FB_PumpRotation → FC_ModbusComm → OB1_Main

**4. ضبط Modbus Communication**
```
في CM 1241 Properties:
- Baud Rate: 19200
- Parity: Even
- Stop Bits: 1
- Data Bits: 8
```

**5. Compile والـ Download**
```
- Compile All: Ctrl+B
- Download to Device
- بعدين Start CPU
```

---

## ملاحظات مهمة

- ضغط الـ Transmitter: **4mA = 0 bar, 20mA = 10 bar**
- Raw value في الـ AI: **4mA = 5530, 20mA = 27648**
- ATV312 Modbus Slave Address: Pump1=1, Pump2=2, ..., Pump5=5
- الـ Rotation كل **72 ساعة** تشغيل فعلي (مش ساعات التشغيل الكلية)
