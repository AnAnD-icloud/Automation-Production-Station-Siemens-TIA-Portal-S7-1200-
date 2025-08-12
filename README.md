🤖 Automation Production Station – TIA Portal (Siemens S7-1200) Project

📌 Overview
A complete mechatronics automation project implemented in Siemens TIA Portal V16 for an Automation Production Station, demonstrating:

🔄 Multi-station coordination

📡 PLC-to-PLC communication (S7 GET/PUT)

🎯 Motion sequencing

🛑 Safety (E-STOP) handling

🖥 HMI/OPC exposure

🗂 Structured PLC software architecture across multiple S7-1200 controllers

This project simulates a real-world automated production cell executing pick-and-place, dispensing, clamping, conveying, inspection, and rejection tasks under a coordinated control strategy. It emphasizes mechanics + sensors + actuators + PLC logic for high-speed, reliable automation training.

🛠 Platform & Tools
💻 Engineering Software: Siemens TIA Portal V16

⚙ PLCs: S7-1200 CPU 1214C DC/DC/DC (6ES7 214-1AG40-0XB0), Firmware V4.0

🌐 Communication: PROFINET (IO Controller) + S7 GET/PUT

📊 HMI/OPC: Tags exposed for HMI, OPC UA, Web API

🖥 OS Used for Engineering: Windows 11 Pro

✨ Key Features
🏭 Multi-station architecture with inter-station data exchange

⏱ Deterministic step sequencing using TON timers

🛑 Safety/E-STOP homing & memory reset logic

🔀 Manual/Auto mode with interlocks

📏 Analog measurement (height check) & decision logic

🗃 Structured tag tables & DBs for HMI integration

🔌 Robust PROFINET options & device replacement support

🗺 Station Topology
🅰 PLC_1 – Station-1 “Automation Production Station” (192.168.0.1)
Core: RPP (rotary pick-and-place), gripper, dispensing, rotation, homing

Blocks: AUTO 1 [FC1], OUTPUT CALL 1 [FC2], ESTOP LOGIC 1 [FC3], DATA BLOCK MOVE [FC4], COM 1 [FC5], Main [OB1]

DBs: DB12, DB13, DB18 for inter-station comms

🅱 PLC_2 – Station-2 “Handling / Conveyor / Inspection” (192.168.0.2)
Tasks: Clamping, conveyor, metal detection, rejection, height check

Analog height check: IW64 → REAL → compare vs threshold

Blocks: AUTO 2 [FC1], OUTPUT CALL 2 [FC2], ESTOP LOGIC 2 [FC3], etc.

🅲 PLC_3 – Future/Auxiliary Station
Prepared comms scaffolding & DB structure for future integration

🧩 Software Architecture
OB1 (Main): Calls station-specific FCs in order

AUTO n: Automatic sequence logic with TON timers

OUTPUT CALL n: Consolidated coil/output control

ESTOP LOGIC n: Homing latch, reset, E-STOP recovery

DATA BLOCK MOVE: IO byte mapping to DBs

COM n: GET/PUT S7 communication with parameter DBs

🌐 Communication Setup
Network: PN/IE_1 → 192.168.0.0/24

Device Names: plc_1, plc_2 (auto PROFINET names also present)

PUT/GET enabled in Protection & Security

Common Heartbeat: %M0.6 (Clock_0.625Hz) as REQ trigger

🔍 Control Logic Highlights
AUTO/MANUAL mode across stations via DB bits

Coordinated start/stop/e-stop signals

RPP Sequence: down → grip → up → rotate CCW → down → release → up → rotate CW → home

Dispensing extend/retract with interlocks

Conveyor & inspection logic with rejection mechanism

Height check decision-making via analog input

📦 Project Structure
PLCs: PLC_1, PLC_2, PLC_3 (scaffold for PLC_4)

Blocks: OBs, FCs, DBs, IEC timers

Tag Tables: IO/state/mode bits for HMI/OPC

🚀 Getting Started
Open in TIA Portal V16.

Assign CPUs & check firmware (V4.0).

Set IP addresses:

PLC_1 → 192.168.0.1/24

PLC_2 → 192.168.0.2/24

Enable PUT/GET in security settings.

Download config & software to PLCs.

Bind DBs to HMI for testing.

Monitor AUTO, OUTPUT CALL, ESTOP logic online.

⚠ Safety Notice
E-STOP implemented in each station.

Always test with proper safety wiring.

For training only – assess risks before real deployment.

📅 Roadmap
📦 PLCSIM Advanced multi-PLC recipe

🖥 HMI screens & faceplates

📢 Diagnostics & alarms

🎛 Parameterizable thresholds via HMI

📚 Documentation & Media
📄 /docs/PART-1.pdf – Full hardware config, block listings, DBs, tag tables.

📷 (Future) Photos & demo videos of station.

👤 Author
Dunna Ananda Sidhvilas
📅 Created: 2025-06-11
🛠 Last Modified: 2025-07-12
