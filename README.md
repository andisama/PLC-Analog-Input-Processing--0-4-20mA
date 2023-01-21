"Processing Analog Input, 0/4-20mA" in Industrial Automation
Towards Industry 4.0. 

See the article at ...


Siemens PLC: SIMATIC S7-1200 Compact PLC, DC/DC/DC
===================================================

- CPU 1214C, compact CPU
- DC/DC/DC
- Onboard I/O: 14 DI 24 V DC; 10 DO 24 V DC; 2 AI 0-10 V DC
- Power supply: DC 20.4-28.8V DC
- Program/data memory 100 KB


Hardware, Software & Wiring Configuration
=========================================

Hardware
- Power Supply
  * 1x External Siemens PSU (Input 220VAC 1 Phase, Output 24VDC).
- CPU
  * 1x Siemens Compact PLC 1200 DC/DC/DC.
- Digital Input
  * 1x Start Push Button(NO) %I0.1.
  * 1x STOP Push Button (NC) %I0.0.
- Analog Input
  * channel 0, %IW64.
  * install 500 Ohm resistor parallel with analog input channel 0 (2x250 Ohm, 2W, 1%).
- Signal Generator
  * 1x Signal Generator to Analog Input Channel 0.
  * only use signal ground (GND) and AIo (output, current).
  * powered by an external USB cable.
  * set the mode on Signal Generator to Voltage (press the mode button to switch between voltage & current).
- Digital Output
  * 5x 24VDC Lamps %Q0.0 (Blue), %Q0.4 (Red), %Q0.5 (Yellow), %Q0.6 (Green), %Q0.7 (White).
- Cabling
  * AWG 16 with ferrules.
- Cabling (color)
  * 220VAC Live: brown, 220VC Neutral: blue.
  * 24VDC+: red, 24VDC common: black.
  * 24VDC Digital Input: white.
  * 24VDC Digital Output: orange.
  * 24VDC Common: black.
  * Analog Input: yellow.
- PLC Firmware level (updated from base 4.5)
  * 4.6.0.
Hardware Configuration change in PLC
- Utilize Realtime Clock (RTC).
  * through TIA portal, devices & network -> system & clock memory -> enable the use of clock memory byte.
  * download to PLC (hardware configuration).
Connectivity
- Laptop to PLC
  * RJ45.
  * straight cable wiring as Siemens 1200 PLC can autocross on the ethernet cable.
Application Software
- Siemens TIA Portal v17 on Windows 11.


PLC Programming
- Ladder Diagram (LD) with Function Block (FB).
- Programming logic.
  * If Start Push Button (NO) is pressed, -> Program is running, and the Start Push Button is latched.
    - on the signal generator, manually use your hand, rotate the button, and the display will indicate 4-20mA.
    - observe that the corresponding output lamps:
      * Red: if the analog signal is between 4-8mA.
      * Yellow: if the analog signal is between 8-12mA.
      * Green: if the analog signal is between 12-16mA.
      * White: if the analog signal is between 16-20mA.
  * 4-20mA analog input from the signal generator is converted by the ADC (within PLC) to 0-27648 (integer).
    - we use NORM_X provided function to normalize this range to 0-100% (real).
    - then, we use SCALE_X provided function to scale the normalized numbers to 4-20mA (real).
  * If the Stop Push Button is pressed, -> Program is stopped.
    - Note that Stop Push Button is NO in the program, but NC in the hardware wiring for safety purposes.
