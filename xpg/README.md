# XPG Controller

## Hardware Specs
| Hardware Revision | MCU | Display | Heater Control MOSFET | Operational Amplifier | UI Input | Supported Soldering Tips |
|-------------|-------------|-------------|-------------|-------------|-------------|-------------|
| v4.0 | Nuvoton NUC029LAN | 2-inch TFT Graphics LCD | Alpha & Omega AO4413 | 3Peak TP1541A | Backlight Push Buttons / Rotary Encoder | C245, C210, T12, 936 |

## Reverse Engineered Resources Available
- XPG v4.0 Schematics (PDF) `XPG_Controller_v4_SCH.pdf`
- XPG v4.0 Schematics (KiCad files)
- XPG v4.0 PCB Layout (PDF)

## Development Resources Available
- XPG v4.0 Pinout Diagram (PDF)
- XPG v4.0 Auto Modes Wiring Guide / Diagrams `XPG_Controller_Auto_Modes_Wiring_Guide.pdf`
- XPG v4.0 GUI Overview `XPG_Controller_GUI_Overview_EN.pdf`
- XPG v4.0 Voltage Regulator Mod (PDF)

## Firmware / Hardware Features
- Supports C245, C210, T12, and 936 soldering tips
- Sleep state detection, which works when the soldering iron is placed in the soldering iron stand/cradle. Sleep detection can also be implemented using a mercury tilt switch fitted inside the soldering iron handle, so when the soldering iron is laid flat on a table or surface, the controller detects it and puts the iron into sleep mode.
- Soldering iron tip change detection. This immediately cuts power to the soldering tip when the controller detects that the user is touching the soldering iron to the stand’s tip-change area.
- Temperature reading adjustment / correction setting (tip-type specific)
- Supports automatic soldering iron handle / tip type detection via ID pin (in some cases, the soldering handle connector may need modification for this function to work)
- Maximum temperature limit setting
- System auto power-off after a user-defined period of time. Requires additional hardware such as relays to function. See `XPG_Controller_Auto_Modes_Wiring_Guide.pdf` for more details.
- Automatic voltage switching according to the connected soldering iron handle / tip type. Requires additional hardware such as relays to function. See `XPG_Controller_Auto_Modes_Wiring_Guide.pdf` for more details.
- Temperature presets
- PID loop variable adjustment setting
- Sleep mode temperature setting
- Buzzer volume adjustment
- Rotary encoder direction correction setting
- Input voltage correction setting
- Multiple GUI color themes
- Room temperature (NTC) correction setting

**Note:** The soldering iron tip change function only works if your soldering iron stand has a dedicated tip-change area/mechanism and is connected to the XPG Controller’s tip change detection pin.  
I personally do not recommend using a mercury tilt switch inside the soldering handle. Instead, I recommend using a soldering iron stand that has a dedicated parking/cradle area for the soldering iron. This also requires the stand to be connected to the XPG Controller’s sleep detection pin.  
Ideally, use a soldering iron stand that has both a soldering handle parking area/cradle and a tip-change area. I recommend the Geeboon SDC02 soldering iron stand.

## Soldering Iron Tip Power Requirements
| Soldering Tip Model | Power Requirement |
|-------------|-------------|
| C245 | 24V, 6A / Max. 144W |
| C210 | 12V, 6A / Max. 72W |
| T12 | 24V, 3A / Max. 72W |
| 936 | 24V, 3A / Max. 72W |

## XPG Controller Power Requirements
- Requires 24V or 12V on the VCC pin (current rating depends on the type of soldering iron being used)
- Can be powered using either an SMPS or a transformer-based circuit
- See `XPG_Controller_Auto_Modes_Wiring_Guide.pdf` for more details

**Note:** I personally recommend using a properly rated toroidal transformer with an additional rectification circuit.

**Warning:** Mains AC is hazardous and can cause severe injury or death. Never work on a live circuit.

## Known Issues
- Linear voltage regulator overheats
- Requires a stable SMPS to operate properly; otherwise, soldering tip temperature may overshoot and sudden shutdowns may occur
- Not compatible with all manufacturers’ soldering tips; some tips may not behave as intended
- PID loop requires calibration; temperature overshooting issues are present
- No documentation is available from the manufacturer. Reverse engineering is currently the only way to understand the hardware and software of this controller
- Soldering tip temperature readings are not very accurate due to the use of a low-cost operational amplifier and less optimized firmware logic
- Not a replacement for commercial JBC-style controllers; overall reliability is lower

## Possible Improvements
- Development of optimized firmware with more robust temperature calculation and control logic
- Use of precision operational amplifiers for more accurate temperature readings and better temperature control
- Redesign of the input power supply circuit using a more efficient and suitable buck converter IC instead of a linear voltage regulator
- Add provision for soldering iron tip grounding (earthing), for example by providing a THT pad on the XPG Controller PCB that can be connected to the soldering iron tip and SMPS earth connection