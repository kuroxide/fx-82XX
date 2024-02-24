![](https://github.com/kuroxide/fx-82XX/blob/master/images/logo.png)
This project contains designs for converting a Casio fx-8200 AU (or similar ClassWiz model calculators) into a general purpose computer, by shoving a pastry inside.

## Rationale
There is no rationale, in fact, this project is very irrationale (just buy a phone) \
The only reason to make this (which is the reason I designed this in the first place) is to troll NESA.

## Design Process
Things are big. Although the Raspberry Pi Zero is tiny, the space inside the calculator is yet tinier. \
Unmodified, there is only about 3mm of vertical clearance to fit stuff inside the case of the calculator, and it is divided up by reinforcing ribs. The Raspberry Pi Zero is barely skinny enough to fit lengthwise across the calculator, and that's before the other components come in.

Thus, we arrive at the winning formula: *Dremel everything.* \
To increase the amount of space available, the ribs are cut out with a dremel tool (you could also use a file if you are very, very dedicated). A custom PCB is used for the keypad; the membrane is re-used since custom making them is a pain in the \[REDACTED] \
The overall design can be split up into 4 parts:
- The keypad
- The power supply
- The display
- The computer itself

### The Keypad
Since the keys and membrane are re-used from the calculator, the only issue is making a custom PCB with a keyboard matrix on it. Vertical clearance is limited, a PCB thickness of 1mm or less is *necessary* to allow everything else to fit. The distances between the keys were measured with a set of calipers and a very good sense of eyeballing, to produce the final matrix layout. \
The Pi's GPIO pins are very useful, especially because there are enough of them to run both a keyboard matrix as well as the display over SPI. The row and column traces on the keypad are directly soldered to the Pi as a HAT (although in this case it's attached on the bottom, so it would be a HAB).

### The Power Supply
BATTERIES. The Pi Zero (and most other SBCs for that matter) run on 5V power, but the AAA cell used by the calculator can provide at maximum around 1.8V. Lithium cells run at 3.7V, which is *allegedly* able to power the Pi, but is definitely not within spec. Therefore, a boost converter has to be used, which solves the voltage issue. \
A Raspbeery Pi Zero 2 averages around 160mAh when idle. Although the main usage (running a calculator or CAS app) is quite light, it also has to power the display, as well as potentially run other programs. Due to the laws of physics, a AAA cell connected to a boost converter is ruled out as an option. Instead, a lithium cell is used. \
Commercially available rectangular LiPo cells are usually 7-10mm thick, which is too obese to fit. However, a 10440 cell is about the same size as a AAA, and can fit in the battery terminals of the calculator (never do this on anything that normally runs off AAAs, by the way). The battery terminals are re-used from the calculator, desoldered and then connected to the 5V boost converter. \
Most 10440 cells can provide somewhere in the neighbourhood of 350mAh. At 3.7V, that works out to 1.295Wh. Assuming that 80% of the capacity is usable, there is about 1Wh available to run the Pi. If the Pi is drawing 250mA@5V (because you're trying to calculate 999999999 factorial or something), the Pi can run for a maximum of 1Wh/1.25W = 0.8 hours. You will want to have more than one calculator if you actually want to use this in an exam (which you shouldn't do, because it's pretty stupid). \
More realistically (sort of), assuming 90% of the capacity is usable and the Pi is drawing 200mA, it can run for 1.1655Wh/1W = 1.1655 hours. If you can find a 10440 cell that *actually* provides more than 350mAh reliably, it will be able to run for longer (obviously). Good luck with that though.

### The Display
There are two options:
1. Reverse engineer the display already in the calculator. This is not a good idea, as will quickly be explained.
2. Replace it with a different one.

Why not option 1? The display is connected to the calculator via a ribbon cable that is glued down and possibly also soldered. It is almost impossible to remove it without damaging it. Also, the pinout is unknown, and reverse-engineering it probably isn't worth the effort. \
Option 2 is relatively much easier, but still quite annoying. Most LCD modules are either too large or too small, don't have the right colour, etc.. The closest thing to a suitable display is a 2.66" e-ink display (covered with some green cellophane to give it the right colour). The display can be purchased either by itself and connected to a driver board, or as a display module with a driver chip already attached.

### The Computer Itself
The PCB layout in this repo is designed for use with a Raspberry Pi Zero W/2/2W, because it is small and relatively powerful, and can have wireless capability. However, it can be easily modified to use most small SBCs. As long as the SBC used can be powered directly from 5V, is under 3mm thick, has SPI pins, and at least 14 GPIOs, it can theoretically be used. Some boards can be made thin enough by desoldering any USB or other ports. The choice of SBC used is a compromise between size, power draw and performance.
Recommendations:
- RPi Zero 2 W for maximum performance, but also maximum power draw
- RPi Zero W / Zero for less performance and less power draw
- MilkV Duo or other small RISC-V boards, for compactness and less power draw but worse support and performance
- Other RPi Zero form factor boards, if they are actually available in your area

## Assembly
Bill of Materials (excl. shipping)
| Part | Cost (AUD) |
| ---- | ---------- |
| Raspberry Pi Zero 2 W | $29.95 |
| Pololu U1V10F5 | $10.05 |
| P-channel MOSFET | $5 for a pack of 10 |
| Keypad PCB | $20 (depends on the fab house) |
| 2.66" Display Module | $25 |
| 10440 Lithium Battery | ~$20 for a pack of 4 |
| Wires | not much |

# Licensing
Code portions of this project are licensed under GPLv3. (See LICENSE) \
This project uses symbols and footprints from the [KiCad libraries](https://www.kicad.org/libraries/)
 <p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://github.com/kuroxide/fx-82XX">fx-82XX</a> by <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://github.com/kuroxide">kuroxide</a> is licensed under <a href="http://creativecommons.org/licenses/by-nc-sa/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">CC BY-NC-SA 4.0<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/nc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/sa.svg?ref=chooser-v1"></a></p> 
