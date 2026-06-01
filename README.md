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
- Releasing START keeps the motor running (Motor latched)
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
<img width="5712" height="3213" alt="IMG_1233" src="https://github.com/user-attachments/assets/1ee75f80-9b8a-437a-8401-53ddec3a7948" />
<img width="5712" height="3213" alt="IMG_1232" src="https://github.com/user-attachments/assets/d3f05d7d-d1d4-48ee-b6ec-4d0685691cd8" />

