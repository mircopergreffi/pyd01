# PYD01 Rev1.0 User Manual

- [Release Notes](./pyd01_rev1.0_notes.txt)
- [Schematics](./pyd01_rev1.0_schematic.pdf)
- [ESPHome Configuration](../../esphome/pyd01_rev1.0_ver1.0.yaml)

## Features
- ESP32-S
- 6 SPST Relays (250V 5A)
- 1 SPST Relay with special internal connection (check schematics for OUT5 and IN5)
- 2 Opto-couplers as output
- 4 Opto-couplers as input
- 1 I2C Header
- 1 SPI Header
- 1 Buzzer
- USB-C Port (programming and serial monitor via CH340G transceiver)

## Fix
An external resistor (680 Ohm) needs to be soldered across the pins of the buzzer.

<img src="pyd01_rev1.0_fix.png" alt="PYD01 Rev1.0 Fix" width="400" />

## Powering the board
The board can be powered via USB or providing an external voltage (7 ~ 28V).
The latter can be useful for application where a power supply is already in use,
or when the user wants to power the board with a battery. In this case, the on-board
DC/DC converter will step down the voltage to 5V in order to power the whole board.

## Relays
The relays can switch loads up to 250VAC/5A and 30VDC/3A (Omron G5NB-1A-E).
This are outputs (OUT3, OUT4, OUT5, OUT7, OUT8, OUT9, OUT10)

Pay attention to the relay connected to the output 5 (OUT5), it is connected directly to the external voltage.
When closed, the external voltage is provided to OUT5. Additionally, input 5 (IN5) can detected
wheter there is a low-resistance path connected to OUT5. This is useful with some doors because they carry
power to the lock mechanism with pins in the door frame, when the door is opened the circuit is broken and IN5
can detect whether the door is opened or closed without additional sensors.

If you want to "claim back" IN5 you have to unsolder diode D10 (check schematics).

## Opto-coupler outputs
The optocouplers outputs can switch up to 35V 5mA and can withstand a reverse voltage of up to 6V.
This are outputs (OUT1, OUT2)

## Opto-coupler inputs
The optocoupler inputs LED has a typical forward voltage of 1.2V and to fully drive it a current of 0.5mA is enough,
but a forward current of IF=1mA is recommended. The maxium absolute forward current is 50mA, but it is recommended to keep it below 5mA.
The following formulas can be used to design the resistor.

The maximum resistance value is:
`R_max[kOhm] = (V_ext[V] - 1.2V) / (IF[mA])`

The minimum resistance value is:
`R_min[kOhm] = (V_ext[V] - 1.2V) / (5mA)`

Then you can choose a resistor with a resistance in between them (higher is preferred).

Last, check that the power the resistor have to dissipate is lower than its rated maximum power. You can calculate the power with the formula:
`P[mW] = (V_ext[V] - 1.2V)^2 / R[kOhm]`

where `R` is the value of the resistor you chose.

## I2C and SPI Headers
The board has two header for digital communication. They are standard headers with a pitch of 2.54mm (0.1 inch).
You can use these to add digital sensors and/or displays.
These headers expose the pins connected to the I2C and HSPI modules of the ESP32-S with two additional pins for 3.3V and ground.
- I2C Header
  - 3.3V
  - SDA
  - SCL
  - GND
- SPI Header
  - 3.3V
  - MISO
  - MOSI
  - CLK
  - CS
  - GND

## Buzzer
On the board is present one buzzer. It is a passive piezo-electric buzzer with the appropriate driving circuit on-board.
It is capable of reproducing tones up to 4kHz.

## GPIOs
The following table summairze all GPIOs and how they are connected on the board.

| Function           | Name  | GPIO | Inverted | Need Pullup\* |
| ------------------ |:-----:|:----:| -------- | ------------- |
| Optocoupler Input  | IN1   | 36   | Yes      | No            |
| Optocoupler Input  | IN2   | 39   | Yes      | No            |
| Optocoupler Input  | IN3   | 34   | Yes      | No            |
| Optocoupler Input  | IN4   | 35   | Yes      | No            |
| Special Input      | IN5   | 25   | Yes      | Yes           |
| Optocoupler Output | OUT1  | 16   | No       | No            |
| Optocoupler Output | OUT2  | 17   | No       | No            |
| SPST Relay         | OUT3  | 32   | No       | No            |
| SPST Relay         | OUT4  | 33   | No       | No            |
| Special Relay      | OUT5  | 26   | No       | No            |
| SPST Relay         | OUT7  | 27   | No       | No            |
| SPST Relay         | OUT8  | 23   | No       | No            |
| SPST Relay         | OUT9  | 18   | No       | No            |
| SPST Relay         | OUT10 | 19   | No       | No            |
| Buzzer             | BUZZ  | 4    | No       | No            |

\* Pullup can be set in software