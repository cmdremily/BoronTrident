# Boron Trident - The Boring Voron Trident
Boring is a positive word in this context. It's meant to imply that owning a Boron Trident is uneventful, no tinkering, just press print, receive part, every time, all the time. 

Specifically, this is the name that I chose to give my particular Voron Trident build inteded to be a work horse. In this repository I will store configuration, part selection, investigations, mods and musings related to this. 

## Status
Work in Progress.

## Design Goals
* Reliability
  * Reduce the number of things that can break.
  * Simplify where possible.
  * Properly rated components and electrical verification.
* Function before form
  * Beauty comes from simplicity, not design.
  * No rainbow vomit LEDs.
* Easy assembly and re-use of BOM from SB2 and Trident
  * Where possible, use items on the BOM of the SB2/Trident that you probably have in your spares drawer.

## List of Current Mods
 Mod | Credit | Purpose | Comment
-|-|-|-
 [Exhaust_cover](https://github.com/VoronDesign/VoronUsers/tree/master/printer_mods/Fiction/Exhaust_cover) | Fiction | Replace back exhaust | Not happy with design. The bottom edge doesn't grab onto the back panel in any way.
 [Boron Motion](motion/README.md) | CmdrEmily | Improve reliability of XY motion through simplification and part selection. | CAN Bus, Umbilical and Sensorless Homing.
 1.5 mm foam | DrachenKätzchen | Simplify BOM. | Not really a mod, use 1.5mm foam instead of 3 mm everywhere. Just replace all 6 mm panel clips with the 4 mm versions from the same STL folder.
 [Bowden Tube Guide](https://github.com/VoronDesign/VoronUsers/tree/master/printer_mods/Galvanic/Bowden_Tube_Guide) | Galvanic | Keep bowden tube and umbilical out of the way | Needs some adjustment to include umbilical. 

## Deviations from Stock BOM / Picks for Generic Parts
I made the following adjustments to the trident BOM.

Category | Function | Part | Comment
-|-|-|-
Frame | Foam | 6mm wide 1.5mm thick foam | The BOM doesn't specify width, but it's 6mm
X-Carriage | E-Axis | Stealth Burner | Do not source parts for the After Burner ("Stelt Boroner" E-axis is WIP).
X-Carriage | Bed Probe | Prusa Super PINDA | Internally temperature compensated, reliable, easy. [Super PINDA Mount](e-axis/SuperPinda/README.md).
X-Carriage | Hotend | Phaetus Dragon HF | High flow for high speed, avoid V6 style groove mount for stability.
X-Carriage | Toolhead PCB | EBB36 | Fits CW2, based on experience from the `#electronics` Voron Discord channel, SBxxxx have many issues.
X-Carriage | Umbilical | [igus CF9.05.04](https://www.igus.ie/product/CF9?artnr=CF9.05.04) | See investigation.
Motion | XYZ Endstops | **None** | Sensorless Homing on X/Y, probe homing on Z.
Build Plate | Alu Plate | [FERMIO Build Plate](https://fermio.xyz/fermio-labs-gmbh/voron-build-plate-350-x-350-mm/) | Claims to be machined from cast and stress relieved aluminium, but lists EN AW 5083 as the material which is susceptible to corrosion past 65°C??
Build Plate | Magnetic Sheet | [GraviFlex Magnetic Sheet 200](https://fermio.xyz/schallenkammer-magnetsysteme-gmbh/schallenkammer-graviflex-magnetic-sheet-200-360-x-360-mm/) | Rated for up to 120°C bed temp sustained.
Build Plate | Bed Heater | [Keenovo](https://fermio.xyz/keenovo-international-group-limited/keenovo-silicone-heatmat-340-x-340-mm-230-v-ac-500-w/) | Power adjusted so it can safely operate at 100%.
Electronics | Main Board | BTT Octopus | Good balance of features and cost.
Electronics | Stepper Drivers | BTT TMC2209 driver boards | TMC2209 has Stallguard and the "default" choice these days.
Electronics | SBC | ODROID C4 | Raspberry Pis are still unobtainium or scalped. The C4 is more power efficient, cheaper and faster. Powered by 12V with internal regulator guarantees voltage stability issues are not a thing.
Electronics | PSU 24V | Meanwell RSP-320-24 | RSP has Active PFC compared to LRS. 320 W allows some headroom and puts typical consumption around 50-60% load where the power efficiency is at its highest.
Electronics | PSU 5V | **None** | The ODROID prefers 12V input. The BTT Octopus has a 12V rail that is mostly unused and comfortably provides enough current.
Electronics | Display | BTT Mini12864 | Chose the BTT version to make wiring easier with main board. Also note that the ODROID C4 doesn't have a DSI interface so you can't use PI-TFT or other high resolution displays unless they support HDMI.
Electronics | 60x25 Fans | Noctua 60x20 12V | They're known for their reliability and quiet operation. As the main board has no way of detecting failed fans, we go for the reliable option. They're thicker than the BOM 60x20 but they fit.
Electronics | Cable Ducts | [3D Printed](https://github.com/VoronDesign/VoronUsers/tree/master/printer_mods/ryandam/Cable_management_duct) | Model by [RyanDam](https://github.com/RyanDam).

## Brand Tier List
In general buy quality brand components, here's some guidance to get started:

Category | Manufacturer | Comment
-|-|-
Fans | Noctua | Top pick when possible. Typically airflow optimized and lower voltages. Check specs. If specs don't line up, check below.
Fans | Sunon | Generally good, well regarded high quality. Commonly counterfeited, buy from reputable seller like DigiKey/Mouser [citation needed].
Fans | Delta | Goto option for server fans where they care only about performance, a bit noisy.
Electronics | BTT | Goto in the community, despite their lackluster documentation and doubious designs at times, still seem to be the best option spec/price wise.
Electronics | Omron | Quality, name brand and well regarded. Costs more, worth it.
Power | Meanwell | Don't skimp on the PSU, if it pops it can take the rest down with it.
