# PLC-Projects
My PLC ladder logic projects built in Do-more Designer as part of self-directed automation engineering study
# PLC Projects — AbdulAzeem Adebayo

Self-directed PLC study using Do-more Designer (AutomationDirect BRX/Do-more).
Built as preparation for JSU's Programmable Logic Controllers course (Fall 2026)
and Summer 2027 engineering internship applications.

## Project 1: Motor Start/Stop with Seal-In Contact
**Platform:** Do-more Designer (BRX simulator)
**Date:** May 2026

### What it does
A motor latching circuit where:
- Pressing START energizes the motor and holds it on via a Motor contact
- Releasing START keeps the motor running (seal-in latched)
- Pressing STOP de-energizes the motor
- Motor does not restart automatically after Stop is released

### I/O List
| Address | Type | Description |
|---------|------|-------------|
| X0 | Input NO | Start pushbutton |
| X1 | Input NC | Stop pushbutton |
| Y1 | Output | Motor contactor |

### Key concept demonstrated
Seal-in contact: the output coil (Y1) uses its own NO contact in parallel with the Start button to maintain the rung true after the momentary Start button is released. This is the fundamental motor control latching circuit used in industrial automation.

### Screenshot
<img width="1920" height="1080" alt="seal-in2" src="https://github.com/user-attachments/assets/063480a2-03ab-4d74-98d2-2c192a57913a" />
<img width="1920" height="1080" alt="seal-in" src="https://github.com/user-attachments/assets/9feba34e-afb9-4a59-8848-3c74b6207a0d" />



## Project 2: Traffic Light Controller
**Platform:** Do-more Designer (BRX simulator)
**Date:** June 2026

### What it does
An automatic traffic light sequence that runs continuously:
- Green light ON for 15 seconds
- Yellow light ON for 5 seconds  
- Red light ON for 15 seconds
- Sequence repeats automatically

### I/O List
| Address | Type | Description |
|---------|------|-------------|
| Y0 | Output | Red light |
| Y1 | Output | Yellow light |
| Y2 | Output | Green light |
| T0 | TMR | Red timer 15s |
| T1 | TMR | Yellow timer 5s |
| T2 | TMR | Green timer 15s |

### Key concepts demonstrated
- Sequential timer logic using TMR instructions
- Self-resetting cycle: T2.Done triggers T0 restart
- Output control using timer Enable and Done bits
- Automatic continuous sequencing without manual input

### Screenshot
<img width="1920" height="1080" alt="traffic light" src="https://github.com/user-attachments/assets/112f6918-de93-492e-a10f-19c16423980a" />
<img width="1920" height="1080" alt="traffic light2" src="https://github.com/user-attachments/assets/1368be70-b0b7-4dfc-b9f6-f49392ce3f7d" />

# Project 3: BRX PLC Up/Down Occupancy Counter

## Overview

This project implements an occupancy counter using an AutomationDirect BRX PLC programmed in Do-more Designer. The system tracks the number of vehicles currently occupying a parking area by counting vehicles entering and exiting through separate sensors.

An Up/Down Counter (UDC) instruction is used to maintain a live occupancy count. The counter increments when a vehicle enters and decrements when a vehicle exits, providing the current number of vehicles in the parking area at any time.

---

## Objectives

* Count vehicles entering the parking area.
* Count vehicles exiting the parking area.
* Maintain a live occupancy value.
* Prevent the occupancy count from exceeding the maximum parking capacity.
* Prevent the occupancy count from decreasing below zero.

---

## System Operation

### Up/Down Counting

A UDC (Up/Down Counter) instruction is used because vehicles both enter and leave the parking area.

* Entry sensor activation increments the counter.
* Exit sensor activation decrements the counter.
* The accumulated count (`.Acc`) represents the current occupancy rather than the total number of vehicles that have passed through the system.

---

### Edge-Triggered Counting

The counter operates on the rising edge (OFF → ON transition) of count input.

This means:

* A vehicle detection signal is counted once when it changes from OFF to ON.
* Holding the sensor ON does not generate additional counts.
* Each transition produces exactly one count event.

This behavior prevents multiple counts from occurring during a single vehicle detection.

---

### Occupancy Limits

The parking area capacity is limited to a predefined maximum value.

The counter's accumulated value (`.Acc`) is monitored using relational comparison instructions:

* The UP count is allowed only when `.Acc < Capacity`.
* The DOWN count is allowed only when `.Acc > 0`.

These conditions prevent:

* The occupancy count from exceeding the parking capacity.
* The occupancy count from becoming negative.

For example, with a capacity of 5 vehicles:

* Occupancy can increase from 0 to 5.
* Additional entry counts are blocked when occupancy reaches 5.
* Exit counts are blocked when occupancy reaches 0.

---

## I/O Mapping

| Address | Description               |
| ------- | ------------------------- |
| X0      | Entry Sensor              |
| X2      | Exit Sensor               |
| X3      | Counter Reset             |
| CT0     | Occupancy Up/Down Counter |

---

## Key PLC Concepts Demonstrated

* Up/Down Counter (UDC) implementation
* Edge-triggered counting
* Relational comparison instructions
* Boundary condition handling
* Occupancy monitoring logic
* bench-simulated project 

---
<img width="1920" height="1080" alt="counter2" src="https://github.com/user-attachments/assets/1b10aa47-328b-4f00-a91f-dcd1b0687ed3" />
<img width="1920" height="1080" alt="counter" src="https://github.com/user-attachments/assets/d73c9e28-7133-4066-a435-f1def201f05c" />


## Testing

The project was tested using the Do-more Designer simulator.

Test cases included:

1. Vehicle entry increments occupancy.
2. Vehicle exit decrements occupancy.
3. Occupancy does not exceed maximum capacity.
4. Occupancy does not decrease below zero.
5. Counter reset returns occupancy to zero.
6. Held inputs generate only one count due to edge-triggered operation.

---

## Future Improvements

* Add direction detection using multiple sensors.
* Display occupancy on an HMI.
* Add "Parking Full" and "Space Available" indicators.
* Log occupancy data for analysis and reporting.
* Integrate physical sensors and hardware testing.

---

## Software Used

* Do-more Designer
* BRX PLC Simulator

---


Controls and Automation Engineering Project

Test 5: Partial Pedestrian Trigger

Sequence:

A person or object activates only S1 and never reaches S2.

Result:

	•	Entering latch set
	•	No confirming second sensor activation
	•	Both sensors eventually clear
	•	Cleanup logic removes the latch
	•	No count generated

Pass.

This test confirms that partial trips do not change the occupancy count.

Test 6: Reversal / Abort

Sequence:

S1 activates, direction is latched, then the vehicle reverses before reaching S2.

Result:

	•	Entering latch set
	•	No confirming second sensor activation
	•	Vehicle leaves the sensor area
	•	Cleanup logic clears the latch
	•	No count generated

Pass.

This test verifies that vehicles which begin a sequence but do not complete it are not counted.

Test 7: Sensor Flicker / Double-Count Protection

Sequence:

A valid entry sequence occurs and increases the count. After the count, an additional sensor activation or flicker occurs.

Result:

	•	Entry counted once
	•	Direction latch consumed and cleared after the count
	•	Additional sensor activity does not generate another count

Pass.

This test verifies that one vehicle movement produces only one count.

⸻

Occupancy Limits

The parking lot capacity is limited to five vehicles.

The PLC only allows an entry count when:

	•	CT0.Acc < 5

The PLC only allows an exit count when:

	•	CT0.Acc > 0

These conditions prevent the counter from exceeding the maximum capacity or going below zero.

⸻

What I Learned

This project showed that counting vehicles is more complicated than simply detecting a sensor turning on.

The main challenge was determining direction while avoiding false counts from partial movements, reversals, and repeated sensor activations. Building the direction latches, count confirmation logic, occupancy limits, and cleanup logic helped me understand how PLC systems use state information to make reliable decisions.

One of the most important lessons was learning to count on the edge of a confirmed event rather than on a condition that remains true for multiple PLC scans. A latched bit can stay on for many scans, which can cause repeated counts if it is not consumed and cleared after use.
<img width="1920" height="1080" alt="phase 3 directional counter2png" src="https://github.com/user-attachments/assets/702cf194-3ee9-4660-928f-0095d49656cf" />
<img width="1920" height="1080" alt="phase 3 directional counter" src="https://github.com/user-attachments/assets/9434e596-91e8-49c7-b7b9-e51640f8c2cc" />


## Author

**AbdulAzeem Adebayo**
