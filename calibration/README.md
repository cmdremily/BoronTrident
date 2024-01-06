# Calibrating a Boron Trident
The Boron Trident is a machine with a high accuracy, in relative terms for an FDM 3D printer. In order to achieve that, some calibration is necessary.

## Step 0: Prerequisites

To complete all the steps below, you will need to make sure that:

* The printer can print.
* The X-, Y-, Z-, and E-axis have reasonable starting values.
    - See the Voron Trident build documentation.
* You have made a copy of the [Boron Trident Calibration Sheet](https://docs.google.com/spreadsheets/d/12_Dv7_rYfVe8zgUhWrPeNcvSJCttsugQXTOSlCp6MAc).
* Have a pair of digital calipers.
* Filament with a very consistent diameter for calibration prints and a profile for it with decent pressure advance calibration.

## Step 1: E-Axis Rotation Distance
Follow [Ellis' Extruder Calibration](https://ellis3dp.com/Print-Tuning-Guide/articles/extruder_calibration.html) procedure but use your digital calipers instead of a ruler. You can chose to use the [Boron Trident Calibration Sheet](https://docs.google.com/spreadsheets/d/12_Dv7_rYfVe8zgUhWrPeNcvSJCttsugQXTOSlCp6MAc) you copied above instead of the one provided by Ellis for convenience.

*N.B. any error in the E-axis `rotation_distance` will be corrected for by per-material extrusion multiplier which you will typically calibrated away anyway. So in a way, this calibration is optional but recommended as having a good value for E-axis allows you to print many materials (less accurately) without calibrating the extrusion multiplier.*

## Step 2: XY-Axis Rotation Distance
Follow the steps in [XY Calibration](xy-differential-calibration.md).

## Step 3: Z-Axis
Lead screws as used on the Z-axis are accurate enough from the factory that no calibration is needed.

## Step 4: Input Shaper
Follow the steps in [Klipper Measuring Resonances](https://www.klipper3d.org/Measuring_Resonances.html#measuring-the-resonances) or [Klipper Resonance Compensation](https://www.klipper3d.org/Resonance_Compensation.html) if no or unreliable ADXL sensor.

## Step 5: Skew Correction
Follow the steps in [Klipper Skew Correction](https://www.klipper3d.org/Skew_Correction.html).

*Note that it's expected that the skew be very small if step 2 above was done well.*

## Step 6: Material Calibration
Even though the XY calibration in step 2 made the XY movement of the printer quite accurate, that alone doesn't produce accurate parts. For each material the following steps should be done:

### Material Shrinkage & Extrusion Multiplier
Material Shrinkage and Extrusion Multiplier needs to be calibrated per filament and nozzle combination.

Follow the steps in [Shrinkage & Extrusion Multiplier Calibration](adv-material-calibration.md).

### Pressure Advance
Pressure advance needs to be calibrated per filament and nozzle combination.

Follow the steps in [Klipper Pressure Advance](https://www.klipper3d.org/Pressure_Advance.html) and/or [Ellis' Pressure Advance](https://ellis3dp.com/Print-Tuning-Guide/articles/pressure_linear_advance/introduction.html).

### Retraction Tuning
Follow the steps in [Ellis Retraction Tuning](https://ellis3dp.com/Print-Tuning-Guide/articles/retraction.html).