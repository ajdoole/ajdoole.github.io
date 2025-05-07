---
title: Human Machine Interface Schematics
tags:
- Human Machine Interface System
---

## Below is the schematics for our HMI subsystem

![Schematic Image](./Screenshot 2025-04-18 151738.png)

## Power Budget

Below is the power budget used to determine the necessary supply voltages for this particular subsystem.

![Power Budget](./2025-02-26.png)


The power budget allows the team to coordinate their micro-controllers with each other. By gathering the information above, team 202 can decide whose board will initiate team power, as well as derive how much power the exhibit will consume. By calculating our consumption we can determine how much power we shall need as a team and how long our exhibit can operate.

In conclusion, the power budget is essential. If one is a part of a team, the team can append the system easier knowing the power consumption of the device being added. Lastly, the budget gives me a quick reference of my power needs and constraints. By estimating my system's maximum power I can choose the most appropriate and capable components to support my system.

## Assembled Board 
![Printed Circuit Board](./pcbfinal.png)

## Version 2.0
To further improve upon this particular design, I would look for ways to shrink the overall size of this board and use a larger OLED screen that would be connected via jumper wires, similarly to how the buttons were attached.  That way, there could be more complex, detailed displays on a larger screen without having to mount it directly onto the board.  Additionally, the orientation of my UART Ribbon cable pins were unaligned with the default arrangement my teammates utilized.  Although this was easily solved with jumper wires, it was not as professional or ideal.  I would also rely on a different microchip, preferably the ESP32 due to its simpler I2C communication messaging and default libraries for OLED applications.  However, the only detail that led this microcontroller to create issues was how there are two pairs of VSS and VDD (power and GND) pins, and one of those pairs were unaligned with my PCB footprint for this component, which resulted in a short in the circuit.  Fortunately, by simply removing those two pins from the copper pads on the board, the short was eliminated.  If I were to use this same type of chip again, of course, I would ensure my custom footprint file is completely accurate to the part's datasheet.
