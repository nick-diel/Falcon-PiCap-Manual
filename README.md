# Unofficial Falcon PiCap Manual
## Version 1.0



## Power Configuration
### PWR Select Jumpers
![image](images/PWR_PI.png?raw=true)
The PiCap can be powered with a 5V input or a 7-24V input.  When using a 7-24V input, the onboard voltage regulator is used to generate a 5v rail for onbord ICs and optionally power the PI itselft (See PWR PI Jumper).  With a 5V input, the onboard voltage regulator is bypassed and the input is used to power the ICs and optionally the Pi.  For both options, the WS2811 outputs are powered directly by the 5V or 7-24V input.  Use the PWR Select jumpers to configure 5V or 7-24V input.  

| Configuration | Jumper Setting | Notes |
| ------------- | -------------- | ----- |
| 5V | Use one jumper on 2 and 3 <br> ![image](images/5V_PWR.png?raw=true) | The onboard voltage regulator is bypassed and everything is directly powered by the 5V input. |
| 7-24V | Use two jumpers: <br>- 1 and 2 <br>- 3 and 4 <br> ![image](images/7-24V_PWR.png?raw=true) | The onboard voltage regulator is used to power the ICs and optionally the Pi, the WS2811 outputs are powered by the 7-24V input.|

### PWR PI Jumper
![image](images/PWR_PI.png?raw=true)

The PiCap gives you the option to power the Raspberry Pi using the power source connected to the PiCap.  This saves you from having to power Raspberry Pi seperately.  If you do choose to power the Pi directly from this PiCap, DO NOT power the Pi from any other souce including other Pi hats or USB cable.

| Configuration | Jumper Setting | Notes |
| ------------- | -------------- | ----- |
| Power the Pi from the PiCap.  Jumper is on. | ![image](images/PWR_PI-Jumper.png?raw=true) | The Pi will receive power directly from the power source connected to the PiCap or via the onboard power supply depending on the power selection.  **Important - Do not power Pi from any other sources; do not plug USB cable in.**|
| Power the Pi seperately from the PiCap.  Jumper is off. | ![image](images/PWR_PI-No-Jumper.png?raw=true) | The Pi will have it's own power source seperate from the PiCap.  You can plug the Pi in with the USB cable.|

Other notes

