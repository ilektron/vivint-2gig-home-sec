# Documentation for the V-DW11-345

## MCU

Contains an MSP430G2452 in the PW Package

Relatively uncomplicated PCB with a PCB antenna, connector for a contact and a reed sensor. 

The PCB only has components on one side and is adhered to the one side of the plastic housing with some doublesided tape. There is a micro-tamper switch and it contains a CR2032 battery.

![Annotated picture of the DW11 sensor PCB](./images/DW11_PCB_ANNOTATED.png)

### Pinout

- Pin 1: DVCC
- Pin 2: AFAIK, NC
- Pin 3: AFAIK, NC
- Pin 4: P1.2 -> Contact
- Pin 5: P1.3 -> Tamper
- Pin 6: Connected to Pin 7 via 47kOhm resistor
- Pin 7: See above
- Pin 8: P1.6 - Seems to be floating
- Pin 9: P1.7 - A7 - ADC - Also seems to be floating, and seems to be used to add jitter to the sensor TX timing to prevent collisions
- Pin 10: !RST/SBWTDIO
- Pin 11: TEST/SBWTCK
- Pin 12: P2.7 -> Reed
- Pin 13: P2.6 -> Data output to the MAX7044 for 345MHz output
- Pin 14: DVSS/GND

### Debugging Connections

As a minimum, connect SBWTDIO and SBWTCK

### Memory Sections

0x02ff to 0x0200 is RAM

0x10ff to 0x1000 is calibration and other info. The fw write some to the 0x1000 to 0x10010 bytes to store between powerups. There is calibration data from 0x10F0 to 0x10FF

#### Info Section
TXID is stored from 0x1003 to 0x1000
Low voltage battery level is stored in 0x1004
A 16 bit seed value is xor'd with 0x0008 and stored in 0x100a
0x1008 stores the event encryption status, where 0 means the legacy honeywell format, 1 means raw, unencrypted data, and 2 means the event data is sent encrypted with the Rabbit cipher. 
0x1009 is some manufacturing counter that also changes the event encryption status.

### FW behavior

The device seems to behave pretty normally and has some extreme low power mode options. It wakes up on a timer, reads pin values, checks if it needs to send data, then goes to sleep.

### Notes from the installation manual

The Vivint Door/Window Contact (DW11) is a sensor that is installed on doors, windows, and other objects in order to monitor open and closed states. The DW11 transmits a signal to the control panel when the magnet is moved away from, or close to, the DW11.

The DW11 device has an external input for NC (Normally Closed) dry contact devices, or it can be used with the supplied magnet directly with the sensor.

The DW11 is also equipped with a cover tamper for additional security

#### Programming Instructions
- Loop 1: Use when the external input is used.
- Loop 2 (default): Use when the magnet is used directly with the sensor.

#### Installer / User Test

Open and/or close the door or window where the DW11 is installed to ensure the sensor is transmitting correctly to the panel. The panel should recognize the state change of the object that is being monitored.

#### Specs

Vivint Part Number (P/N) V-DW11-345
Model Number (M/N) DW02
Wireless Signal Range 350 ft. (106.7 m), open air
Battery Panasonic CR2032 or equivalent lithium battery
Battery Life 3-5 years (normal usage)
Transmitter Frequency 345 MHz
Code Outputs Open, Close, Tamper, Low Batt., Loss of Supervision
Supervisory Interval 70 minutes per signal (12 hours for panel to report supervision failure)

