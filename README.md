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
| Y0 | Output | Green light |
| Y1 | Output | Yellow light |
| Y2 | Output | Red light |
| T0 | TMR | Green timer 15s |
| T1 | TMR | Yellow timer 5s |
| T2 | TMR | Red timer 15s |

### Key concepts demonstrated
- Sequential timer logic using TMR instructions
- Self-resetting cycle: T2.Done triggers T0 restart
- Output control using timer Enable and Done bits
- Automatic continuous sequencing without manual input

### Screenshot
<img width="1280" height="720" alt="traffic light pic" src="https://github.com/user-attachments/assets/2d41ec19-9287-4081-aeeb-60d3d1169397" />
<img width="1280" height="720" alt="traffic light" src="https://github.com/user-attachments/assets/de30d3e9-da9b-43e1-9df3-930b88681c9f" />

