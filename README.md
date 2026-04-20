# Water Station Control System (PLC & SCADA)

## Project Overview
This project implements a fully automated control system for a municipal water station serving a residential city. The system manages five water pumps—four active and one standby—to maintain line pressure based on demand. The control logic is developed for the **Siemens S7-1200 PLC** using TIA Portal, featuring Modbus RTU communication with **Schneider ATV312** variable frequency drives (VFDs).

## Technical Specifications
* **Controller:** SIMATIC S7-1200 CPU.
* **VFDs:** 5x Schneider Electric Altivar 312 (ATV312).
* **Communication:** Modbus RTU (RS485) for Speed Setpoints and Status Monitoring.
* **Instrumentation:** 4-20mA Pressure Transmitter (0-10 Bar range).
* **Software:** TIA Portal (PLC), WinCC (SCADA), EPLAN (Electrical Design), and Node-RED (Dashboard).

## System Functionality

### 1. Operation Modes
* **Manual Mode:** Pumps are controlled via physical Start/Stop buttons at a fixed frequency.
* **Automatic Mode:** The system uses a pressure-based staging logic to maintain target pressure.

### 2. Pressure Staging Logic (Automatic)
The system monitors a 0-10 Bar pressure transmitter and scales the number of active pumps accordingly:
* **7.5 to 10.0 Bar:** 1 Pump Active
* **5.0 to 7.5 Bar:** 2 Pumps Active
* **2.5 to 5.0 Bar:** 3 Pumps Active
* **0.0 to 2.5 Bar:** 4 Pumps Active
* **VFD Control:** In Auto mode, pump speeds are dynamically adjusted via Modbus to reach and maintain the pressure setpoint.

### 3. Fault Tolerance & Standby Logic
* **Redundancy:** One pump is always designated as **Standby**.
* **Fault Recovery:** If any active pump triggers an **Overload** or **Fault** signal, the logic automatically engages the Standby pump to ensure service continuity.

### 4. Runtime Balancing (Rotation)
To ensure equal wear and tear across the equipment:
* **Interval:** Every 72 operating hours, the system performs an automatic rotation.
* **Logic:** The pump with the highest accumulated hours is stopped, and the current Standby pump is brought into the active rotation.
* **Persistence:** Hour counters are retained during power cycles and transitions between Manual and Automatic modes.

## Project Structure
* **/TIA_Portal:** Contains the S7-1200 project, including FBs for Rotation Logic, Modbus Mapping, and Pressure Scaling.
* **/EPLAN:** Electrical schematics for the control panel, including I/O wiring and VFD power circuits.
* **/WinCC_SCADA:** The HMI/SCADA interface featuring real-time pressure trends, alarm logging, and pump status indicators.
* **/Dashboard:** A modern web-based visualization (Node-RED/HTML) for remote monitoring.

## Setup & Installation
1.  **PLC:** Load the station configuration into TIA Portal. Ensure the Modbus MB_MASTER block is configured for the correct COM module parameters (Baud rate, parity, and stop bits).
2.  **VFDs:** Configure Schneider ATV312 parameters for Modbus control (Set `TCC` to 2-wire, `tCT` to LEL, and `nAd` to unique slave IDs 1 through 5).
3.  **SCADA:** Run the WinCC Runtime. Ensure the PG/PC interface is set to the correct Ethernet adapter to communicate with the S7-1200.

## Future Improvements
* Implementation of a Lead-Lag PID controller for smoother pressure transitions.
* Energy consumption monitoring via Modbus power registers.
* SMS/Email alerts for critical system faults using a communication processor.
