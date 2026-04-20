# EPLAN Panel Layout - Water Station Control Panel
## MCC (Motor Control Center) - 5 Pump Station

---

## أبعاد اللوحة
```
Cabinet Type: Free-standing
Width:   1000mm (2 sections × 500mm)
Height:  2200mm
Depth:   600mm
IP Rating: IP54
```

---

## الـ Section الأول (500mm) - Power Section

### الشريط العلوي (من أعلى لأسفل):

```
┌─────────────────────────────────────┐
│  MAIN CIRCUIT BREAKER               │  400V 3P, 63A
│  (Schneider iC60N)                  │
├─────────────────────────────────────┤
│  MAIN SURGE PROTECTION              │  SPD Type 2
│  (Schneider IPRD40r)                │
├─────────────────────────────────────┤
│  MCPCB - P1 (Motor Protection CB)   │  16A, 3P+N
│  MCPCB - P2                         │  16A
│  MCPCB - P3                         │  16A
│  MCPCB - P4                         │  16A
│  MCPCB - P5                         │  16A
│  MCPCB - CTRL (Control Circuit)     │  6A, 1P
├─────────────────────────────────────┤
│  ATV312 Drive - Pump 1              │  7.5kW (حسب الموتور)
│  ATV312 Drive - Pump 2              │  
│  ATV312 Drive - Pump 3              │  
│  ATV312 Drive - Pump 4              │  
│  ATV312 Drive - Pump 5              │  
└─────────────────────────────────────┘
```

---

## الـ Section الثاني (500mm) - Control Section

### الشريط العلوي (من أعلى لأسفل):

```
┌─────────────────────────────────────┐
│  S7-1214C CPU                       │  على DIN Rail
│  CM 1241 RS485                      │  Modbus
│  SM 1231 AI 4ch                     │  4-20mA Input
│  SM 1221 DI 16ch                    │  24VDC DI
│  SM 1222 DO 16ch                    │  24VDC DO
├─────────────────────────────────────┤
│  24VDC SMPS - 10A                   │  Schneider ABL8
├─────────────────────────────────────┤
│  Terminal Blocks - WAGO 281 Series  │
│  (Power + Signal + Modbus)          │
└─────────────────────────────────────┘
```

### Door (باب اللوحة):

```
┌──────────────────────────────────────────────┐
│    WATER STATION CONTROL PANEL               │
│    محطة مياه - لوحة تحكم                    │
│                                              │
│   ┌──────────────────────────────────┐       │
│   │  AUTO ○────────────────○ MANUAL  │  S0   │
│   │        SYSTEM MODE               │       │
│   └──────────────────────────────────┘       │
│                                              │
│   PUMP 1          PUMP 2          PUMP 3     │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐  │
│  │ ● RUN   │    │ ● RUN   │    │ ● RUN   │  │
│  │ ● FAULT │    │ ● FAULT │    │ ● FAULT │  │
│  │ AUTO○MAN│    │ AUTO○MAN│    │ AUTO○MAN│  │
│  │ [START] │    │ [START] │    │ [START] │  │
│  │ [STOP ] │    │ [STOP ] │    │ [STOP ] │  │
│  └─────────┘    └─────────┘    └─────────┘  │
│                                              │
│   PUMP 4          PUMP 5                     │
│  ┌─────────┐    ┌─────────┐                 │
│  │ ● RUN   │    │ ● RUN   │    ● SYS ALARM  │
│  │ ● FAULT │    │ ● FAULT │                 │
│  │ AUTO○MAN│    │ AUTO○MAN│    [FLT RESET]  │
│  │ [START] │    │ [START] │                 │
│  │ [STOP ] │    │ [STOP ] │    [E-STOP]     │
│  └─────────┘    └─────────┘                 │
│                                              │
│   PRESSURE: [   6.5  BAR  ]                 │
│   (Analog Panel Meter 0-10 bar)              │
└──────────────────────────────────────────────┘
```

---

## قائمة المواد (Bill of Materials)

| الكمية | الوصف | الموديل | الشركة |
|--------|-------|---------|-------|
| 1 | Main Circuit Breaker 63A 3P | iC60N C63 3P | Schneider |
| 1 | Surge Protection Device | IPRD40r 3P+N | Schneider |
| 6 | MCPCB 16A 3P | GV2ME16 | Schneider |
| 5 | VFD Drive 7.5kW | ATV312HU75N4 | Schneider |
| 1 | S7-1214C DC/DC/DC | 6ES7214-1AG40-0XB0 | Siemens |
| 1 | CM 1241 RS485 | 6ES7241-1CH32-0XB0 | Siemens |
| 1 | SM 1231 AI 4×13bit | 6ES7231-4HD32-0XB0 | Siemens |
| 1 | SM 1221 DI 16×24VDC | 6ES7221-1BH32-0XB0 | Siemens |
| 1 | SM 1222 DO 16×24VDC | 6ES7222-1BH32-0XB0 | Siemens |
| 1 | SMPS 24VDC 10A | ABL8REM24100 | Schneider |
| 1 | Selector Switch 3-pos | XB4BD33 | Schneider |
| 10 | Selector Switch 2-pos (Start/Stop) | XB4BD21 | Schneider |
| 5 | Selector Switch 2-pos (Auto/Man) | XB4BD21 | Schneider |
| 10 | Pilot Light Green 24VDC | XB4BVB3 | Schneider |
| 10 | Pilot Light Red 24VDC | XB4BVB4 | Schneider |
| 1 | Pilot Light White 24VDC | XB4BVB1 | Schneider |
| 1 | Emergency Stop 40mm | XB4BS8445 | Schneider |
| 1 | Analog Panel Meter 0-10 bar | - | - |
| 1 | Pressure Transmitter 4-20mA | FCX-AII | Fuji/Yokogawa |
| 50m | Control Cable 1.5mm² | - | - |
| 50m | Power Cable 4mm² | - | - |
| 20m | Modbus Cable (Shielded 2-pair) | - | Belden |

---

## ملاحظات EPLAN

**Naming Convention:**
- Pages: =WS+CAB1/1 (Power), =WS+CAB1/2 (Control), =WS+CAB1/3 (IO)
- Cables: WS-P01-001 (Power Pump 1), WS-C01-001 (Control), WS-MB-001 (Modbus)
- Tags: P01-M01 (Motor 1), P01-D01 (Drive 1), PLC-01 (S7-1200)

**Wire Colors:**
- L1/L2/L3: Black/Brown/Grey
- Neutral: Blue
- Earth: Green/Yellow
- 24VDC+: Red
- 24VDC-: Blue with Black stripe
- Signal: White
- Modbus: Purple (twisted pair shielded)
