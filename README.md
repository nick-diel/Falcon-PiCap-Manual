# Unofficial Falcon PiCap Manual
**Version 0.2**

This is the unofficial manual for the Falcon PiCap.  It is not associated with PixelController, LLC, David Pitts, or Falcon Christmas.  This information is provided as is with no warranty, the authors and contributors or anyone else connected with this manual are not responsible for your use of the information contained or linked in this manual, use at your own risk.

The Falcon PiCap is a Raspberry Pi HAT that can control two strings of WS2811 pixels, output DMX/Renard/LOR, and has a real-time clock.  It is designed to be used with Falcon Player ([Falcon Christmas](https://falconchristmas.com/)).

This manual is specific to the board designed and sold by David Pitts at [PixelController](https://www.pixelcontroller.com).  While there may be other versions sold by other parties, this is the only recommended board.  The reasons are as followed:
* While the actual circuit design is not overly complex, the well designed PCB layout of this board is critical when dealing with large currents.  This is not only to prevent show failures, but is a safety issue as well.
* For the same reasons above, this board has high quality, correctly specced components.  Other boards may have questionable components from unknown origins.
* The developers of Falcon Player specifically support this board
* David Pitts donates 50% of proceeds of sales of the Falcon PiCap to the Falcon Player Development team.

**[Purchase the Falcon PiCap here](https://www.pixelcontroller.com/store/index.php?id_product=47&controller=product)**

## Falcon PiCap Overview
![image](images/Falcon_PiHat_Overview.png?raw=true)

## Power Configuration
The Falcon PiCap has two power settings that need to be configured prior to use.  First you need to select if you are using a 5V or a 7-24V input.  Second you need to configure if you will be powering the Raspberry Pi from the Falcon PiCap.


### Selecting 5V or 7-24V Input
![image](images/PWR_Select.png?raw=true)

The PiCap can be powered with a 5V input or a 7-24V input.  When using a 7-24V input, the onboard voltage regulator is used to generate a 5v rail for the onboard ICs and optionally power the PI itself (See Powering the Raspberry Pi).  With a 5V input, the onboard voltage regulator is bypassed and the input is used to power the ICs and optionally the Pi.  For both options, the WS2811 outputs are powered directly by the 5V or 7-24V input.  Use the PWR Select jumpers to configure 5V or 7-24V input.

| Input | Jumper Setting | Notes |
| ----- | -------------- | ----- |
| 5V | One jumper on 2&3 <br> ![image](images/5V_PWR.png?raw=true) | The onboard voltage regulator is bypassed and everything is directly powered by the 5V input.  **If this jumper configuration is selected ensure your input is not greater then 5V, else you risk damaging the PiCap and the Raspberry Pi** |
| 7-24V | Two jumpers on: <br>- 1&2 <br>- 3&4 <br> ![image](images/7-24V_PWR.png?raw=true) | The onboard voltage regulator is used to power the ICs and optionally the Pi, the WS2811 outputs are powered by the 7-24V input.|

### Powering the Raspberry Pi
![image](images/PWR_PI.png?raw=true)

The PiCap gives you the option to power the Raspberry Pi using the power source connected to the PiCap.  This saves you from having to power the Raspberry Pi separately.  If you do choose to power the Pi directly from this PiCap, DO NOT power the Pi from any other source including other Pi hats or the USB cable.

| Configuration | Jumper Setting | Notes |
| ------------- | -------------- | ----- |
| Power the Pi from the PiCap| Jumper On <br> ![image](images/PWR_PI-Jumper.png?raw=true) | The Pi will receive power directly from the power source connected to the PiCap or via the onboard power supply depending on the power selection.  **Important - Do not power Pi from any other sources; do not plug the USB cable in.**|
| Power the Pi separately from the PiCap | Jumper Off <br> ![image](images/PWR_PI-No-Jumper.png?raw=true) | The Pi will have it's own power source separate from the PiCap.  With this option, it is safe to plug the Pi in with the USB cable.|

### Other Notes
* When powering the Raspberry Pi with the PiCap (PWR PI jumper on), take caution as you are bypassing some of the power protection circuit of the Raspberry Pi.
* The 7-24V input has a polarity protection diode, the 5V input does not.  Use extra caution to ensure the correct polarity when hooking up a 5V power source.
* The onboard voltage regulator appears to be an OKI-78SR or some variant of it.  The datasheet can be found [here](https://power.murata.com/pub/data/power/oki-78sr.pdf).
* The F3 fuse, protecting the onboard components and optionally the Raspberry Pi, is rated for 2A.  However, (when using the 7-24V input) the onboard voltage regulator is rated for 1.5A and the voltage regulator may fail prior to the fuse blowing.
* The OKI-78SR is rated for 7-36V.  This may mean if your PiCap has this voltage regulator and you want to void the warranty, take on additional risks, you might be able to power 36V lights with the PiCap.  You might need to replace the input filter cap (C3) with one that is rated for 72V or higher (2 x input voltage).  There may be other design constraints that would limit using an input voltage greater then 36.

## Real-Time Clock
The real-time clock chip and battery provide the Pi a clock that keeps time when the Pi has no power.  This is a useful feature for running shows based on schedules.  If you do not need this feature, you do not have to have the battery installed to use the PiCap's other outputs.

The battery contact takes a CR1225 battery and is installed with the positive (+)side up, negative (-) side toward the circuit board. 

The battery should last numerous years (in theory 10 years) while installed.  If you want, you can remove it when not using the PiCap for extended periods, though that may not extend the battery life much as the battery breaks down naturally anyways.

## DMX/LOR/REN Output
The PiCap has one RJ-45 output that can be configured for DMX, Light-O-Rama, or Renard.

To configure the output, use three jumpers on the row indicated for the desired output (DMX, LOR, or Renard).  For example, to select DMX output, place three jumpers on the top row.


In addition, there is a jumper to enable a 120Ω termination resistor.  In most cases this would be used, if you have a case where it is not needed you can remove this jumper.

![image](images/DMX_Selected.png?raw=true)

### Pin Assignment
| Output | Data +<br>(non-inverted)| Data -<br>(inverted)| GND |
| ------ | ------ | ------ | --- |
| **DMX** | Pin 1 | Pin 2 | Pin 7 |
| **LOR** | Pin 4 | Pin 5 | Pin 6 |
| **REN** | Pin 5 | Pin 5 | Pin 2|

## Raspberry Pi Pin Usage
Note - when using WS2811 outputs, you cannot use GPIO 18 or 19.

| Pin Number | Pin Name | Pin Usage |
| ---------- | -------- | --------- |
| 3 | SDA | I2C data line to communicate with real-time clock chip (clock address is 0x68) |
| 4 | SCL | I2C clock line to communicate with real-time clock chip |
| 8 | TXD | Serial transmit, used for DMX/LOR/REN output |
| 10 | RXD | Serial receive, connected to the differential transceiver, but would not actually be used |
| 12 | GPIO 18 | WS2811 Output 1, uses PWM0 |
| 35 | GPIO 19 | WS2811 Output 2, uses PWM1 |
| 2, 4 | 5.0V | Connected when PWR PI jumper is used |
|6, 9, 14, 20, 25, 30, 34, 39 | GND | Connected to PiCap's ground |

