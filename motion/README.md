# Boron Motion (Work in Progress)

The Boron Trident changes the Voron Trident motion system with the below mods aiming to improve reliability and simplicity:

* CAN Bus
* Umbilical
* Sensorless Homing for XY
* Probe homing for Z

This means that we do not use any microswitches for endstops.

## Why do the Umbilical mod?
From lurking in the Voron discord, I've observed that the breakage of cables in the drag chains is a quite common failure mode. As of May 2023, the sourcing guide and BOM for the Voron Trident only specs "High-Flex Wire" with a minimum strandcount, leaving the builder to their devices. However, just because a cable is high flex doesn't mean that it's suitable for use in drag chains.

Manufacturers like igus and Lapp make cables that are rated specifically for cable chains, they are stiffer to take a larger bend radius in the drag chain among other things. But that's not explained to the builder in the BOM, sourcing guide or manual.

*By using an umbilical we distribute the bend of the cables over the entire length of the umbilical, producing a much lower strain on the cable making it last much longer, even when compared to properly rated cables.*

In addition, removing the dragchain does the following:
* Removes moving components that can cause issues.
* Reduces the moving mass in the X/Y motion system. On average half the cable chains mass must be accelerated during tool-head moves.

##  Why do the CAN Bus mod?
The CAN Bus mod is pretty much mandatory for the umbilical mod. Having all the wires that normally go to a toolhead ia long umbilical will get unwieldly and hard to maintain. It trades wiring complexity for configuration complexity, and wire failures to PCB failures. However, the configuration complexity is a one time cost and the PCB failure is much less likely than individual, non-drag-chain-rated wires. So on the whole it's a boost for reliability at the cost of configuration.

Common problems with CAN Bus, seem to stem from user error at install time or bad choice of cable/mounting. With properly rated cable, correct mounting (supplied mods) and install these issues are mitigated.

Using CAN Bus also reduces mass in the X/Y motion system by the mass of wires being removed.

## Why do the Sensorless Homing mod?
The most common criticisms of Sensorless Homing is that it's not as accurate, or simple as dedicated endstops. The endstop accuracy is needed to be able to correctly position the nozzle over the Z endstop, if the nozzle is not above the Z endstop, then the bed can potentially crash into the toolhead. Crash is bad.

*Note: The accuracy loss of using Sensorless Homing only affects the home position, it doesn't affect print accuracy.*

I argue that while a endstop is simpler to understand and to configure than Sensorless Homing, it's not simpler in terms of the hardware build. We're trading homing accuracy and configuration burden for simplicity and fewer things that can break. If you're worried about the reliability of sensorless homing, consider that the Prusa MK2/MK3 printers which are known for their reliability use sensorless homing on all axes.

We mitigate the issue of the inaccurate home position and the crash risk of the Z axis by using an always connected Super PINDA (see the [e-axis mods](../e-axis/README.md)) inductive bed probe as a virtual Z endstop both for homing and for kinematic bed tramming. The Prusa Super PINDA probe is also highly reliable, again, they used on the Prusa MK3 printers known for their reliability. 

The downside is that we're limited to metallic print sheets with the inductive probe. However spring steel sheets are pretty much the standard these days so that's not really a big issue in practice.

## BOM
This is a total BOM list for the motion system mods. The BOM items are repeated in steps 1 and 2 for convenience and in case you only want to do those mods. The mods here rely on the SuperPINDA being installed from the [e-axis mods](../e-axis/README.md).

**Do not substitute the Micro-Fit 3.0 connectors for any other connector types!** MicroFit 3 are chosen for their current carrying capability, secure connections and relatively small size. There is a cloned Micro-Fit 3.0 2x2 plug included in the EBB36 of "jmconn" brand. If you're not comfortable with this clone (the header on the PCB has no markings but is presumably also a clone, they should be fine), add a Micro-Fit 3.0 43020-series 2x2 plug to the BOM below as replacement as well as 4x MicroFit 3.0 43030 series crimp sockets.

**Do not substitute the CF9.05.04 cable**, it has been carefully chosen to have long service life in this application and be workable without special tools. Yes, it's expensive for a piece of cable, properly rated cables always are.

**The below quantities are minimum quantities!** Get extras, especially of crimp connectors.

Quantity | Item | Comment
-|-|-
1x | BTT EBB36 | With included accesories.
3 m | [CF9.05.04 cable from igus](https://www.igus.ie/product/CF9?artnr=CF9.05.04) with 4x0.5mm^2 conductors (do not pick 4G0.5, the cables aren't colour coded) | Do not draw more than 4A (96W@24V) from the toolhead for prolonged periods of time.
2x | M12x1.5 Cable Strain Relief | Make sure threads are M12x1.5 and that it's for 6.5mm cable. Example `LAPP 53111700`.
2x | MicroFit 3.0 43020 series - 1x4 in-line plug | Trident BOM. Alternatively 2x2 can be used for a cleaner look but they're not part of Trident BOM.
2x | MicroFit 3.0 43025 series - 1x4 in-line receptacle | See above comment.
8x | MicroFit 3.0 43030 series - crimp socket | Trident BOM. Very easy to mess up crimping if you're not used to it, get extras. 
8x | MicroFit 3.0 43031 series - crimp pin | See above comment.
1x | RJ11/12 plug | For connecting CAN Bus to BTT Octopus.
0.5 m | Red 0.5 mm^2 (AWG20) Stranded Hook-up Wire | Trident BOM. Length depending on wiring situation in electronics bay, safe amount indicated.
0.5 m | Black 0.5 mm^2 (AWG20) Stranded Hook-up Wire | See above comment.
2x 0.5 m | 0.14 mm^2 to 0.25 mm^2 (AWG26-AWG24) Solid core Hook-up wire | Any two unique colors, preferrably not black or red. Length depending on wiring situation in electronics bay, safe amount indicated.
2x | M5x16 BHCS (ISO 7380-1) | Trident BOM.
1x | M5x10 BHCS (ISO 7380-1) | Trident BOM.
1x | M3x6 BHCS (ISO 7380-1) | Trident BOM.
2x | M3x40 SHCS (DIN912)  | Trident BOM. 
4x | M3x12 SHCS (DIN912)  | Trident BOM. 
2x | M3x8 SHCS (DIN912)  | Trident BOM. 
4x | M3 Washer (DIN125) | Trident BOM.
1x | M3 Threaded Insert | Trident BOM.

## How mod?

These steps are for a stock Voron Trident with Stealth Burner/Clock Work 2, adapt to your situation.

### Step 0 - Disconnect Wires and Endstops
1. Disconnect tool-head wire harness from tool-head and remove it completely from the printer.
1. Unscrew XY Endstop Pod (replace the two M3x30 SHCS bolts with two M3x16 SHCS bolts), undo the cabling and unplug it from the controller board.
1. Remove the Z endstop from the bed frame, undo the cabling and unplug it from the controller board.
1. Remove drag chain anchor from CW2.

### Step 1 - Install the mount for fixed end of the umbilical

Follow the steps in [Umbilical Motor Mount](umbilical-motor-mount/README.md).

### Step 2 - Install the mount for the moving end of the umbilical

Follow the steps in [Umbilical Extruder Mount](umbilical-extruder-mount/README.md).

### Step 3 - Install umbilical cable
1. Pull one of the strain reliefs over one end of the CF9 cable. Strip and crimp that end of the cable to connect with the EBB36 on the X-Carriage. Pay close attention to which cables are 24V, GND, CAN_H and CAN_L. Make note maybe even label them.
2. Measure out the right length of CF9 cable to the [umbilical motor mount](umbilical-motor-mount/README.md), make sure you have enough extra to be able to mess up the crimping a few times. Thread through the strain relief on the umbilical motor mount, and pull through enough cable to get just below the A motor. Crimp and attach 2x2 MicroFit 3 female connector.
   * We create a joint with MicroFit 3 connectors after the strain relief so that the umbilical can be removed without rewiring the electronics bay should service be required. The bit after the strain relief will not be moving and is highly unlikely to break, hence we can tie it down nice and clean to the vertical extrusion by the A motor. Match wire colours all the way through.
3. Use the rest of the CF9 cable to run down to the electronics bay. Crimp the end by the A motor, make sure wire colours matches in the connectors. 
4. Connect 24V and GND to the 24V power supply. Use a multimeter to check between PSU studs and 12V rail/GND on the EBB36, if you get either of these two wires wrong, you will likely have to buy a new EBB36.
5. Use WAGO spring clamp terminals to connect CAN_H on the CF9 cable to CAN_H on the RJ12 connector and same for CAN_L. We use WAGO here until we're sure they're correct after configure clipper. Once sure it's correct, replace with male/female 1x2 MicroFit 3 connection.

### Step 4 - Configure clipper
