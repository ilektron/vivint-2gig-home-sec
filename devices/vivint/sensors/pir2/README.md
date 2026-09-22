
# Documentation for the V-PIR2-345

Uses the MSP430FE251 chip

## Pinout

- Pin 10: TEST/SBWTCK
- Pin 11: !RST/SBWTDIO
- Pin 13: P2.6 -> LED
- Pin 14: P2.7 -> User Button
- Pin 17: P1.1 -> Sensitivity high
- Pin 18: P1.2 -> Pet immunity low
- Pin 19: P1.3 -> UTXD0
- Pin 20: P1.4 -> URXD0
- Pin 21: P1.5 -> Tamper SW
- Pin 22: P1.6 -> Data Out 
- Pin 23: P1.7 -> P1.7/UCLK0/TA1/TDO/TDI
- Pin 24: P2.0 -> P2.0/STE0/TA0/TDI/TCLK 

## Debug Interface

- Pin 1: URXD0
- Pin 2: VCC
- Pin 3: TEST
- Pin 4: !RST
- Pin 5: GND
- Pin 6: UTXD0

## 0xD0 packet


From the SVSCTL register, VLDx is as follows

```
0001b = 1.9 V
0010b = 2.1 V
0011b = 2.2 V
0100b = 2.3 V
0101b = 2.4 V
0110b = 2.5 V
0111b = 2.65 V
1000b = 2.8 V
1001b = 2.9 V
1010b = 3.05 V
1011b = 3.2 V
1100b = 3.35 V
1101b = 3.5 V
1110b = 3.7 V
```
 
## Notes from the installation manual

### Installer Test

Hold the test button for 2 seconds to send test signals to the control panel.

### User Test

1. Press and hold the test button on the side of the device. The red LED should turn on as soon as the button is pressed. After 2 seconds, the red LED will blink once to indicate the device has entered test mode.
2. The red LED will briefly turn on every few seconds that motion is detected. The PIR2 will also transmit to the panel while motion is detected. NOTE: Test mode lasts for 90 seconds, after which the device returns to normal operation.

### Specifications

Wireless Signal Range 350 feet (106.7 m), open air
Batteries 2 X Panasonic CR123A or equivalent
Battery Life 3-5 years under normal usage
Transmitter Frequency 345 MHz
Code Outputs Alarm, Alarm Restore, Supervisory, Low Battery, Tamper
Supervisory Interval 70 minutes per signal (12 hours for panel to report supervision failure)
Operating Temperature Limits 32° to 120°F (0° to 49°C)
Relative Humidity 5-95% Non-Condensing



