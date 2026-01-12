# TREB – Autonomous Beacon-Finding Robot (VEX Cortex)

## Project Overview

TREB (short for *Trebuchet*) is an autonomous mobile robot developed as part of **ENGR 10: Introduction to Engineering** at **San José State University**. The robot was designed to simulate an assistive household robot capable of:

1. Locating and turning off a **red infrared beacon** (representing a stove)
2. Locating and securing a **green infrared beacon** (representing a plate)
3. Exiting the arena while transporting the green beacon

The robot integrates **mechanical design**, **custom electronics (IR board)**, and **embedded software** written in **ROBOTC for VEX Cortex**.

---

## Hardware Configuration

* **Platform:** VEX Cortex (SquareBot base)
* **Drive Motors:** 2× VEX 393 motors (differential drive)
* **Arm Mechanism:** Servo-driven trebuchet-inspired swinging arm
* **Sensors:**

  * Custom **IR Receiver Board** with 7 photodetectors
  * Front limit switch (beacon contact detection)
  * Rear left & right bumpers (arena exit navigation)
* **Beacon Frequencies:**

  * Red beacon: **1 kHz**
  * Green beacon: **10 kHz**

---

## Software Environment

* **Language:** C (ROBOTC dialect)
* **IDE for deployment:** ROBOTC for VEX Cortex
* **Local editing/storage:** VS Code (optional)

> Note: The code must be compiled and downloaded to the robot using **ROBOTC**. VS Code is used only for editing and version control.

---

## Control Architecture

The robot is implemented using a **state machine**, allowing clear separation of behaviors and easier debugging.

### States

1. **FIND_RED** – Uses IR photodetectors to locate the red beacon
2. **PRESS_RED** – Activates the arm repeatedly until the red beacon is turned off
3. **BACKOUT_FROM_RED** – Reverses to create clearance
4. **FIND_GREEN** – Switches IR frequency and locates the green beacon
5. **SECURE_GREEN** – Lowers arm to hold the green beacon
6. **EXIT_ARENA** – Uses rear bumpers to navigate out of the arena

State transitions are event-driven (sensor triggers rather than time-based when possible).

---

## Beacon Navigation Logic

Beacon navigation is adapted from the provided **GOTO BEACON** framework and consists of:

* `Read_PD()` – Reads values from 7 photodetectors
* `find_max()` – Identifies the photodetector with strongest IR signal
* `move_step()` – Steers the robot toward the beacon using proportional control

If no beacon signal is detected, the robot performs a search spin.

---

## Arena Exit Logic

The robot exits the arena using a **reactive bumper-based strategy**:

* Left bumper pressed → rotate counterclockwise
* Right bumper pressed → rotate clockwise
* Both bumpers pressed → drive straight forward quickly (failsafe)

This logic ensures reliable exit regardless of robot orientation.

---

## File Structure

```
TREB_Robot/
├── treb_state_machine.c   # Main ROBOTC program
├── README.md              # Project documentation
└── images/                # Robot photos / diagrams (optional)
```

---

## How to Run the Code

1. Open **ROBOTC for VEX Cortex**
2. Set:

   * Robot → Platform Type → VEX Robotics
   * Robot → Platform Subtype → VEX Cortex
3. Open `treb_state_machine.c`
4. Compile the program
5. Connect the robot via USB and power it on
6. Download to robot and press **Run**

---

## Key Learning Outcomes

* Embedded C programming with real-time sensor feedback
* State machine design for autonomous robotics
* Integration of mechanical, electrical, and software subsystems
* Debugging hardware–software interaction under time constraints

---

## Authors

**Aarya Sukhadiya** and project team members
San José State University – Charles W. Davidson College of Engineering

---

## Notes

This repository represents a reconstructed and refactored version of the original project code, based on preserved documentation and reports. The functional behavior matches the original competition-tested robot while improving readability and maintainability.
