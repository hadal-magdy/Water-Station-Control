# WinCC SCADA Configuration
## Water Station - Tag List & Screen Design

---

## Tag List

### System Tags
| Tag Name | Data Type | PLC Address | Description |
|----------|-----------|-------------|-------------|
| SYS_MasterAuto | Bool | DB_SystemConfig.bMasterAuto | وضع التشغيل |
| SYS_Pressure_Bar | Real | DB_SystemConfig.rPressure_Bar | الضغط الحالي |
| SYS_RequiredPumps | Int | DB_SystemConfig.iRequiredPumps | طلمبات مطلوبة |
| SYS_ActivePumps | Int | DB_SystemConfig.iActivePumps | طلمبات شغالة |
| SYS_StandbyPump | Int | DB_SystemConfig.iStandbyPump | رقم الـ Standby |
| SYS_Fault | Bool | DB_SystemConfig.bSystemFault | فولت عام |
| SYS_PID_Output | Real | DB_SystemConfig.rPID_Output | خرج الـ PID |
| SYS_TargetPressure | Real | DB_SystemConfig.rPressure_Target | الضغط المستهدف |

### Pump Tags (مكررة ×5 - بدّل الرقم)
| Tag Name | Data Type | PLC Address |
|----------|-----------|-------------|
| P{n}_Running | Bool | DB_PumpData.Pump[n].bIsRunning |
| P{n}_Fault | Bool | DB_PumpData.Pump[n].bIsFault |
| P{n}_Standby | Bool | DB_PumpData.Pump[n].bIsStandby |
| P{n}_AutoMode | Bool | DB_PumpData.Pump[n].bAutoMode |
| P{n}_Speed | Real | DB_PumpData.Pump[n].rActualSpeed |
| P{n}_SpeedSP | Real | DB_PumpData.Pump[n].rCurrentRamp |
| P{n}_RunHours | Real | DB_PumpData.Pump[n].rRunningHours |
| P{n}_HoursRotation | Real | DB_PumpData.Pump[n].rHoursSinceRotation |
| P{n}_Current | Int | DB_PumpData.Pump[n].iMotorCurrent |
| P{n}_Freq | Int | DB_PumpData.Pump[n].iOutputFreq |
| P{n}_StartCount | Int | DB_PumpData.Pump[n].iStartCount |

---

## Screens Design

### Screen 1: Main Overview (الشاشة الرئيسية)
```
┌─────────────────────────────────────────────────────────┐
│  محطة مياه - Water Station Control         [Date/Time]  │
│  ─────────────────────────────────────────────────────  │
│                                                         │
│   PRESSURE GAUGE      SYSTEM STATUS                     │
│   ┌──────────┐       Mode: [AUTO/MANUAL]                │
│   │  ⊙ 6.5  │       Active Pumps: 2/5                  │
│   │   bar    │       Standby: Pump 5                    │
│   └──────────┘       Target: 8.0 bar                   │
│                      PID Output: 65%                    │
│                                                         │
│   PUMPS STATUS                                          │
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐         │
│  │  P1  │ │  P2  │ │  P3  │ │  P4  │ │  P5  │         │
│  │ RUN  │ │ RUN  │ │STOP  │ │STOP  │ │ STB  │         │
│  │ 65%  │ │ 65%  │ │  -   │ │  -   │ │  -   │         │
│  │12.5h │ │ 8.2h │ │ 5.1h │ │ 3.4h │ │ 0.0h │         │
│  └──────┘ └──────┘ └──────┘ └──────┘ └──────┘         │
│                                                         │
│  [Pump Detail] [Trends] [Alarms] [Settings]            │
└─────────────────────────────────────────────────────────┘
```

**عناصر الشاشة:**
- **Pressure Gauge:** WinCC فيها Bar Graph + Numeric Display + Color zones:
  - 0-2.5 bar = أحمر
  - 2.5-7.5 bar = برتقالي
  - 7.5-10 bar = أخضر
- **Pump Status:** لكل طلمبة:
  - دايرة خضرا لما Running
  - دايرة حمرا لما Fault
  - دايرة رمادية لما Stop
  - مثلث أصفر لما Standby
  - عداد الساعات تحتيها

---

### Screen 2: Pump Detail
```
┌─────────────────────────────────────────────────────────┐
│  Pump 1 Details                              [Back]     │
│  ─────────────────────────────────────────────────────  │
│                                                         │
│  Status: ● RUNNING          Mode: AUTO                 │
│                                                         │
│  Speed:    [████████░░] 65.0%    50.0 Hz               │
│  Current:  12.5 A                                       │
│  Run Hours: 125.3 h                                     │
│  Starts:    48                                          │
│  Since Rotation: 24.5 h / 72.0 h                       │
│                                                         │
│  ┌── Speed Trend (last 30 min) ───────────────────┐    │
│  │  100%│                                          │    │
│  │   75%│    ───────────────                       │    │
│  │   50%│ ─/                                       │    │
│  │   25%│/                                         │    │
│  │    0%└──────────────────────────────            │    │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  [Reset Fault]  [Force Stop]  [Pump 2 ►]               │
└─────────────────────────────────────────────────────────┘
```

---

### Screen 3: Trends
- **Pressure Trend:** آخر 24 ساعة - Line Chart
- **Active Pumps:** Bar Chart كل ساعة
- **Speed Trend:** للطلمبات الشغالة
- Archive Rate: كل 1 ثانية للضغط، كل 10 ثواني للسرعة

---

### Screen 4: Alarms
```
Alarm Configuration:
- HIGH_PRESSURE:   رسالة + صوت + أحمر    | > 9.5 bar
- LOW_PRESSURE:    رسالة + صوت + أحمر    | < 0.5 bar
- PUMP1_FAULT:     رسالة + صوت + حمرا    | P1 bIsFault = TRUE
- PUMP2_FAULT:     ...                    | P2 bIsFault = TRUE  
- PUMP3_FAULT:     ...                    | P3 bIsFault = TRUE
- PUMP4_FAULT:     ...                    | P4 bIsFault = TRUE
- PUMP5_FAULT:     ...                    | P5 bIsFault = TRUE
- SYS_FAULT:       رسالة + صوت + حمرا    | bSystemFault = TRUE
- ROTATION_DONE:   معلومة                | bRotationDone = TRUE
- COMM_ERROR:      تحذير                 | iLastFaultCode ≠ 0
```

**Alarm Report:** Export كل يوم تلقائي إلى Excel

---

## Events Log
```
Event Tags:
- Mode Change (Auto↔Manual)
- Pump Started / Stopped
- Fault Occurred / Cleared
- Rotation Executed
- Setpoint Changed
```
