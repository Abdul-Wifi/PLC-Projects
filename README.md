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
Seal-in contact: the output coil (Y1) uses its own NO contact in 
parallel with the Start button to maintain the rung true after 
the momentary Start button is released. This is the fundamental 
motor control latching circuit used in industrial automation.

### Screenshot
<img width="5712" height="3213" alt="IMG_1235" src="https://github.com/user-attachments/assets/e443b162-a9c4-4d75-97c9-dcd6b482aaa6" />
<img width="5712" height="3213" alt="IMG_1234" src="https://github.com/user-attachments/assets/9756d3ab-da89-4443-99c3-a777e50c2e73" />

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
<img width="1280" height="720" alt="traffic light pic" src="https://github.com/user-attachments/assets/2d41ec19-9287-4081-aeeb-60d3d1169397" />
<img width="1280" height="720" alt="traffic light" src="https://github.com/user-attachments/assets/de30d3e9-da9b-43e1-9df3-930b88681c9f" />

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
<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 9 15 51 PM" src="https://github.com/user-attachments/assets/7196bbd4-c3e8-4878-bc71-e159da837858" />
<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 9 16 21 PM" src="https://github.com/user-attachments/assets/ab09517f-ce9d-4c31-9816-21c81e518736" />


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

## Author

**AbdulAzeem Adebayo**

Controls and Automation Engineering Project

