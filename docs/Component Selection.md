
## Table 1: OLED Screen

| Component                                                                                                    | Image                                                                                                             | Voltage Range (V) | Max Current (mA) | Pros                                                                                         | Cons                                                                |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|-------------------|------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| Newhaven Display Intl NHD-C12832A1Z-FSW-FBW-3V3 Cost: $12.78/unit [datasheet](https://newhavendisplay.com/content/specs/NHD-C12832A1Z-FSW-FBW-3V3.pdf) | ![OLED Display](https://github.com/user-attachments/assets/cd7eb6ce-0679-4ae2-a178-05703a275109) [Link](https://www.digikey.com/en/products/detail/newhaven-display-intl/NHD-C12832A1Z-FSW-FBW-3V3/2059236) | 2.7 - 3.3         | 100              | Inexpensive, PSOC Compatible, Visually clear, Larger Screen                                    | Smaller voltage input, Limited display options                      |
| CANADUINO® 26295 Cost: $7.68/unit [datasheet](https://universal-solder.ca/downloads/canaduino_oled_1.3_i2c.pdf) | ![OLED Display](https://raw.githubusercontent.com/ajdoole/ajdoole.github.io/refs/heads/main/OLED2.jpg) [Link](https://www.digikey.com/en/products/detail/universal-solder-electronics-ltd/26295/16822118) | 2.7 - 3.3         | 100              | Inexpensive, PSOC Compatible, Visually clear, Fewer Pins                                       | Smaller voltage input, Smaller Screen                               |

Part Chosen: Option 2
--
# Rationale:
The CANADUINO OLED screen had fewer pins to work with and was a more universal option for most electrical projects.  It made the task easier to adapt to and provide a clear display on a more compact screen.  Additionally, its only task was to display small text; nothing too complex or vivid, merely a few lines of sentences for the user to read when using the HMI system.

## Table 2: Microcontroller

| Component                                                                                                    | Image                                                                                                             | Voltage Range (V) | Max Current (mA) | Pros                                                                                         | Cons                                                                |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|-------------------|------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| PIC18F55Q43-E/PT Cost: $1.29/unit [datasheet](https://ww1.microchip.com/downloads/en/DeviceDoc/PIC18F25-45-55Q43-Data-Sheet-40002170E.pdf) | ![PIC Microcontroller](https://raw.githubusercontent.com/ajdoole/ajdoole.github.io/refs/heads/main/docs/mc2.jpg) [Link](https://www.digikey.com/en/products/detail/microchip-technology/PIC18F55Q43-E-PT/13622188) | 1.8 - 5.5         | 5.8              | Universal Peripheral Options, low power consumption, UART, SPI/I2C Support                   | Limited RAM, low speed computing, Not ideal for 32-bit processing   |
| PIC18F47Q10-I/MP Cost: $1.73/unit [datasheet](https://ww1.microchip.com/downloads/en/DeviceDoc/PIC18F27-47Q10-Data-Sheet-40002043E.pdf) | ![PIC Microcontroller](https://github.com/user-attachments/assets/3af3f4a4-dec5-4eca-b5ae-b35ef0282502) [Link](https://www.digikey.com/en/products/detail/microchip-technology/PIC18F47Q10-I-MP/10064342) | 1.8 - 5.5         | 5.8              | 16-bit pipeline, PSOC Compatible, UART, SPI/I2C Support                                       | Limited RAM, Limited processing power, Small Flash Memory           |

Part Chosen: Option 1
--
# Rationale:
The first PIC microchip is more compact and has a familiar pin layout to other popular models, such as the PICTQFP44, and can operate at lower voltage levels, in this case our desired limit of 3.3V.

## Table 3: Voltage Regulator

| Component                                                                                                    | Image                                                                                                             | Voltage Range (V) | Max Current (mA) | Pros                                                                                         | Cons                                                                |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|-------------------|------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| 3.3 - 5 V Regulator MAX8563EEE Cost: $1.26/unit [datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/max8563-max8564a.pdf) | ![Voltage Regulator](https://github.com/user-attachments/assets/85d63ddf-a8a7-4c06-8473-265e24f89ef8) [Link](https://www.digikey.com/en/products/detail/analog-devices-inc-maxim-integrated/MAX8563EEE/12615195) | 5                 | 100              | DC-DC step down, Adjustable Voltage output, 2.2MHz Switching Frequency                        | Limited Output Current, Requires external inductor                  |
| LM2575D2T-3.3R4G Cost: $3.32/unit [datasheet](https://www.onsemi.com/pdf/datasheet/lm2575-d.pdf) | ![Voltage Regulator](https://raw.githubusercontent.com/ajdoole/ajdoole.github.io/refs/heads/main/14-SOIC.jpg) [Link](https://www.digikey.com/en/products/detail/onsemi/LM2575D2T-3-3R4G/1476688) | 3.3               | 100              | AC-DC step down, Adjustable Voltage output, 3.3V Output                        | Limited Output Current                                             |

Part Chosen: Option 2
--
# Rationale:
The reason for choosing this switching regulator is due to the low amount of external components and constant 1A current output as the rest of the system does not need more than ""mA.

## Table 4: Push-Buttons

| Component                                                                                                    | Image                                                                                                             | Voltage Range (V) | Max Current (mA) | Pros                                                                                         | Cons                                                                |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|-------------------|------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| PR144C1900 Cost: $2.32/unit [datasheet](https://mm.digikey.com/Volume0/opasdata/d220001/medias/docus/5621/ESCH_S_A0005378353_1-2548291.pdf) | ![Button](https://raw.githubusercontent.com/ajdoole/ajdoole.github.io/refs/heads/main/docs/buttons2.jpg) [Link](https://www.digikey.com/en/products/detail/e-switch/PR144C1900/2116178) | 3                 | 100              | Universal compatibility, Compact, UART, Long lifespan                                        | Limited current handling, Quantity depends on overall functionality |
| D6R90 F2 LFS Cost: $1.56/unit [datasheet](https://www.ckswitches.com/media/1341/d6.pdf) | ![Button](https://github.com/user-attachments/assets/42778357-858b-40ed-8ae8-fab6e264a9e7) [Link](https://www.digikey.com/en/products/detail/c-k/D6R90-F2-LFS/1466352) | 3                 | 100              | Universal compatibility, Compact, UART                                                       | Limited current handling, Quantity depends on overall functionality |

Part Chosen: Option 1
--
# Rationale:
Despite their size in comparison to the other major components, these buttons can easily be soldered to male-to-female jumper wires and connected to male header pins on the board.
