Automation Production Station – TIA Portal (Siemens S7-1200) Project

A complete mechatronics automation project implemented in Siemens TIA Portal V16 for an Automation Production Station, demonstrating multi-station coordination, PLC-to-PLC communication (S7 GET/PUT), motion sequencing, safety (E-STOP) handling, HMI/OPC exposure, and structured PLC software architecture across multiple S7‑1200 controllers.
Overview

This project simulates a real-world automated production cell with multiple stations executing pick-and-place, dispensing, clamping, conveying, inspection, and rejection tasks under a coordinated control strategy. It emphasizes seamless interaction of mechanics, sensors, actuators, and PLC logic to train students and professionals on high-speed, reliable automation.
•	Platform: Siemens TIA Portal V16
•	PLCs: S7‑1200 CPU 1214C DC/DC/DC (6ES7 214-1AG40-0XB0), Firmware V4.0
•	Communication: PROFINET (IO Controller), S7 GET/PUT PLC-to-PLC
•	HMI/OPC: Tags exposed for HMI/OPC UA/Web API
•	OS used for engineering: Windows 11 Pro

Features
•	Multi-station architecture with inter-station data exchange
•	Deterministic step sequencing using timers (TON) and memory bits
•	Safety/E-STOP homing and memory reset logic
•	Manual/Auto mode with common start/stop/estop interlocks
•	Analog measurement handling and simple process evaluation (e.g., height check)
•	Structured tag tables and DBs for HMI integration
•	Support for device replacement and robust PROFINET options

Station Topology
Three primary PLCs (with support blocks for expansion):
•	PLC_1 (192.168.0.1): Station‑1 “Automation Production Station”
o	Core operations such as RPP (rotary pick-and-place), gripper open/close, dispensing extend/retract, rotation CW/CCW, homing
o	Blocks: AUTO 1 [FC1], OUTPUT CALL 1 [FC2], ESTOP LOGIC 1 [FC3], DATA BLOCK MOVE [FC4], COM 1 [FC5], Main [OB1]
o	Inter-station DBs: STATION-1 INPUT AND OUTPUT DATA [DB12], STATION 2 INPUT OUTPUT DATA [DB13], STATION‑3,4,5 INPUT [DB18]
o	Uses S7 GET/PUT via GET_DB/PUT_DB instances
o	I/O example: DI14/DQ10 onboard, AI2(0–10V) at IW64/IW66
•	PLC_2 (192.168.0.2): Station‑2 “Handling/Conveyor/Inspection”
o	Clamping and handling cylinder motion (extend/down/up/retract), conveyor run/stop, metal detection and rejection sequence, height check with AI
o	Blocks: AUTO 2 [FC1], OUTPUT CALL 2 [FC2], ESTOP LOGIC 2 [FC3], DATA BLOCK MOVE [FC4], COM 2 [FC5], Main [OB1]
o	Inter-station DBs: STATION 1 INPUT OUTPUT [DB15], STATION 3 INPUT OUTPUT [DB16], STATION 2 INPUT OUTPUT [DB17]
o	Analog height check: IW64 converted to REAL (%MD70) and compared against threshold
o	GET/PUT with dedicated DBs
•	PLC_3 (future/aux station scaffold)
o	Communication scaffolding (DATA BLOCK MOVE, COM 3, Main)
o	Tag/DB structure prepared for integration (e.g., with Station‑1/2)

Notes:
•	Additional pages include PLC_4 scaffolding content for OUTPUT CALL/ESTOP patterns, enabling future scale-up.
Software Architecture
•	OB1 (Main): Calls functional blocks in a defined order for each PLC
•	AUTO n (FC): Automatic sequence logic per station (interlocks, step timing with TON)
•	OUTPUT CALL n (FC): Centralized coil/output driving grouped by step/state bits
•	ESTOP LOGIC n (FC): Homing latch, reset, memory housekeeping, E‑STOP recovery timers
•	DATA BLOCK MOVE (FC): Consistent mapping of process IO bytes to inter-station DBs
•	COM n (FC): S7 communication using GET/PUT with dedicated parameter DBs (GET_DB, PUT_DB variants)
•	IEC Timers: DB2..DB12+ instances for each TON stage

Communication
•	Network: PN/IE_1 on 192.168.0.0/24
•	Device names: plc_1, plc_2 (auto-generated PROFINET names also present)
•	S7 GET/PUT enabled in Protection & Security; PUT/GET allowed from remote partner
•	GET/PUT parameter DBs: GET_DB, GET_DB_2/3/4, PUT_DB, PUT_DB_1/2/3 etc.
•	Common heartbeat: Clock_0.625Hz (%M0.6) used as REQ trigger in communication blocks

Control Logic Highlights
•	Mode selection: AUTO/MANUAL propagated across stations via shared DB bits
•	Common start/stop/e‑stop networked to all stations to ensure coordinated behavior
•	RPP sequence (Station‑1): down → grip → up → rotate CCW → down → release → up → rotate CW → home
•	Dispensing extend/retract with interlocks and TON timing
•	Station‑2 clamping/HCM (handling cylinder) extend/down/up/retract with staged TON delays and confirmation sensors
•	Conveyor control based on start conditions, position sensors, and inspection outcomes
•	Metal rejection: trigger extend, timed retract, and conveyor stop logic
•	Height check: analog read (IW64) → REAL conversion → compare vs setpoint (e.g., 7,000.0) → decision bits
I/O and Tags
•	Digital Inputs: I0.0–I1.7 typical; 6.4ms filter default
•	Digital Outputs: Q0.0–Q1.1 typical; substitute value disabled on STOP
•	Analog Inputs: IW64/IW66, 0–10V, 50Hz integration, weak smoothing
•	Clock/System bits: MB0/MB1; FirstScan %M1.0, AlwaysTRUE %M1.2
•	Tag tables: Default tag tables for each PLC expose IO, state, and mode bits for HMI/OPC

Project Structure
•	TIA Portal V16 project with:
o	PLCs: PLC_1, PLC_2, PLC_3 (and scaffold for PLC_4)
o	Program blocks (FC/OB), system IEC timer DBs
o	Data Blocks for station IO and inter-station comms
o	PLC tag tables and default tag sets
o	Traces/measurements scaffolds included for future diagnostics

Getting Started
1.	Open the project in Siemens TIA Portal V16.
2.	Assign the target CPUs (S7‑1200 1214C DC/DC/DC) and ensure firmware compatibility (V4.0).
3.	Set IP addresses:
o	PLC_1: 192.168.0.1/24
o	PLC_2: 192.168.0.2/24

o	Adjust additional PLCs as needed
5.	Enable PUT/GET communication and confirm the same in Protection & Security.
6.	Download the hardware config and software to each PLC.
7.	Use the Station DBs (DB12/DB13/DB15/DB16/DB17/DB18, etc.) for HMI bindings and manual testing.
8.	In Online mode, monitor AUTO n, OUTPUT CALL n, and ESTOP LOGIC n networks to validate sequences.

Safety
•	ESTOP logic is implemented in each station, including homing and timed resets.
•	Always validate safety wiring and hardwired E‑STOP circuits on real hardware.
•	This project is for educational/training; conduct a full risk assessment before real-world deployment.

Requirements
•	Siemens TIA Portal V16
•	S7‑1200 CPUs (1214C DC/DC/DC) or virtual simulation with TIA PLCSIM Advanced
•	Windows 10/11 engineering station
•	Optional: HMI/OPC UA/Web API client for tag access

How It Works (Quick Walkthrough)
•	Operator selects AUTO/MANUAL and presses START (DB bits).
•	AUTO 1 runs RPP and dispensing sequence using timers and sensor confirmations.
•	OUTPUT CALL 1 consolidates outputs (gripper, rotate, extend) from memory/state bits.
•	PLC_2 receives Station‑1 flags via GET, runs clamping/conveyor/inspection, and returns results via PUT.
•	Height and metal inspection outcomes determine rejection and conveyor stop behavior.
•	ESTOP at any station triggers homing and resets using dedicated FC and TON DBs.

Folder/Block Naming Conventions
•	AUTO n: Automatic sequence per station
•	OUTPUT CALL n: Output/coil consolidation
•	ESTOP LOGIC n: E‑STOP homing + reset
•	DATA BLOCK MOVE: IO Byte mapping to shared DBs
•	COM n: GET/PUT data exchange
•	STATION-x INPUT/OUTPUT: HMI-facing DBs
•	IEC_Timer_0_DB_*: Timer instances for TON

Roadmap
•	Add PLCSIM Advanced recipe for multi-PLC simulation
•	Add HMI screens and faceplates for each station
•	Add diagnostics/alarm text lists
•	Implement technology objects (PTO/PWM/HSC) where needed
•	Parameterize thresholds (e.g., height) via HMI setpoints

Screenshots/Docs
•	Exported PDF includes full hardware config, block listings, DB structures, and tag tables for PLC_1..PLC_3.
Author
•	Author: DUNNA ANANDA SIDHVILAS
•	Last modified by: MTC-MECH-05
•	Creation time: 2025-06-11
•	Last change: 2025-07-12

License
This project is provided for educational and research purposes. Adapt, extend, and use at own risk. For commercial deployment, ensure compliance with local safety standards and perform a formal validation.

Acknowledgments
•	Siemens TIA Portal engineering environment and SIMATIC S7‑1200 platform
•	Thanks to contributors and reviewers who validated the station logic and communication flows

How to Contribute
•	Fork the repo, create a feature branch, commit changes with clear messages, and open a pull request.
•	Include updated PDFs or screenshots for changed blocks/DBs and a brief test plan.

Contact
For questions or collaboration, open an issue in the repository with:
•	PLC target details (CPU type/firmware)
•	TIA Portal version
•	Brief description of the scenario and observed behavior

Happy automating!
