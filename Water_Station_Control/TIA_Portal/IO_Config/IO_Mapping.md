# IO Mapping - Water Station Control
## S7-1214C + Expansion Modules

---

## Digital Inputs - SM 1221 DI 16×24VDC
**Slot 3 | Base Address: I0.0**

| Address | Signal Name | Description | Type |
|---------|-------------|-------------|------|
| I0.0 | MASTER_AUTO_SEL | Main Auto/Manual Selector | NO |
| I0.1 | P1_MAN_SEL | Pump 1 Manual Selector | NO |
| I0.2 | P1_START | Pump 1 Start Button | NO |
| I0.3 | P1_STOP | Pump 1 Stop Button | **NC** |
| I0.4 | P1_OVERLOAD | Pump 1 Overload Relay | **NC** |
| I0.5 | P2_MAN_SEL | Pump 2 Manual Selector | NO |
| I0.6 | P2_START | Pump 2 Start Button | NO |
| I0.7 | P2_STOP | Pump 2 Stop Button | **NC** |
| I1.0 | P2_OVERLOAD | Pump 2 Overload Relay | **NC** |
| I1.1 | P3_MAN_SEL | Pump 3 Manual Selector | NO |
| I1.2 | P3_START | Pump 3 Start Button | NO |
| I1.3 | P3_STOP | Pump 3 Stop Button | **NC** |
| I1.4 | P3_OVERLOAD | Pump 3 Overload Relay | **NC** |
| I1.5 | P4_MAN_SEL | Pump 4 Manual Selector | NO |
| I1.6 | P4_START | Pump 4 Start Button | NO |
| I1.7 | P4_STOP | Pump 4 Stop Button | **NC** |
| I2.0 | P4_OVERLOAD | Pump 4 Overload Relay | **NC** |
| I2.1 | P5_MAN_SEL | Pump 5 Manual Selector | NO |
| I2.2 | P5_START | Pump 5 Start Button | NO |
| I2.3 | P5_STOP | Pump 5 Stop Button | **NC** |
| I2.4 | P5_OVERLOAD | Pump 5 Overload Relay | **NC** |
| I2.5 | FAULT_RESET | Global Fault Reset Button | NO |

---

## Digital Outputs - SM 1222 DO 16×24VDC
**Slot 4 | Base Address: Q0.0**

| Address | Signal Name | Description |
|---------|-------------|-------------|
| Q0.0 | P1_RUN_LAMP | Pump 1 Run Lamp (Green) |
| Q0.1 | P2_RUN_LAMP | Pump 2 Run Lamp (Green) |
| Q0.2 | P3_RUN_LAMP | Pump 3 Run Lamp (Green) |
| Q0.3 | P4_RUN_LAMP | Pump 4 Run Lamp (Green) |
| Q0.4 | P5_RUN_LAMP | Pump 5 Run Lamp (Green) |
| Q0.5 | P1_FAULT_LAMP | Pump 1 Fault Lamp (Red) |
| Q0.6 | P2_FAULT_LAMP | Pump 2 Fault Lamp (Red) |
| Q0.7 | P3_FAULT_LAMP | Pump 3 Fault Lamp (Red) |
| Q1.0 | P4_FAULT_LAMP | Pump 4 Fault Lamp (Red) |
| Q1.1 | P5_FAULT_LAMP | Pump 5 Fault Lamp (Red) |
| Q1.2 | SYS_ALARM | System General Alarm (Red) |
| Q1.3 | AUTO_LAMP | Auto Mode Indicator (White) |

---

## Analog Inputs - SM 1231 AI 4×13bit
**Slot 2 | Base Address: IW64**

| Address | Signal Name | Range | Description |
|---------|-------------|-------|-------------|
| IW64 | PRESSURE_RAW | 4-20mA | Pressure Transmitter 0-10 bar |

**تحويل الضغط:**
```
Raw = 5530  → 0 bar   (4mA)
Raw = 27648 → 10 bar  (20mA)
Formula: P(bar) = (Raw - 5530) / 22118 × 10
```

---

## Communication - CM 1241 RS485
**Slot 101**

| Parameter | Value |
|-----------|-------|
| Protocol | Modbus RTU |
| Baud Rate | 19200 bps |
| Parity | Even |
| Data Bits | 8 |
| Stop Bits | 1 |
| Termination | ON (آخر جهاز في الـ Bus) |

**Modbus Addresses للـ Drives:**
```
ATV312 Pump 1 → Slave Address 1
ATV312 Pump 2 → Slave Address 2
ATV312 Pump 3 → Slave Address 3
ATV312 Pump 4 → Slave Address 4
ATV312 Pump 5 → Slave Address 5
```

**ضبط ATV312 للـ Modbus:**
```
- COM → Add → SLA = 1 (رقم الـ Slave)
- COM → Add → tbr = 19.2 (Baud Rate)
- COM → Add → tFo = Even (Parity)
- CfG → CTL = Mdb (Control Mode = Modbus)
- CfG → Fr1 = Mdb (Frequency Reference = Modbus)
```
