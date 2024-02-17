# Material Shrinkage and Extrusion Multiplier Tuning

## Why is This Calibration Necessary?
Following the mechanical precision achieved with [XY Differential Calibration](xy-differential-calibration.md), the next phase in achieving dimensionally accurate prints is to fine-tune material-specific parameters. This guide outlines the process for calibrating material shrinkage and extrusion multiplier to ensure your prints match the intended dimensions accurately.

## How is This Different from Other Calibration Procedures?
This calibration stands out by using a differential measurement strategy that eliminates the influence of errors in the extruder's `rotation_distance` and the extrusion multiplier set in the slicer. This precision allows for an accurate determination of the material shrinkage factor, which in turn informs a more correct extrusion multiplier for the filament.

For a detailed explanation, refer to the accompanying [paper](shrinkage-and-multiplier-calibration.pdf).

## Prerequisites
Ensure you have:
- Completed the [XY Differential Calibration](xy-differential-calibration.md).
- Accurately [calibrated your extruder's `rotation_distance`](https://ellis3dp.com/Print-Tuning-Guide/articles/extruder_calibration.html).
- Calibrated [pressure advance](https://ellis3dp.com/Print-Tuning-Guide/articles/pressure_linear_advance/introduction.html) for the filament, with a slight overestimate to prevent corner bulging.
- Familiarized yourself with the "Material Calibration" tab in the [calibration spreadsheet](https://docs.google.com/spreadsheets/d/12_Dv7_rYfVe8zgUhWrPeNcvSJCttsugQXTOSlCp6MAc).
- Performed temperature and stringing tuning as temperature changes post-calibration can affect shrinkage.
- Access to precise measurement tools:
    * 150 mm digital calipers are required for the shrinkage calibration.
    * A micrometer is required for the extrusion width calibration presented here (±0.1% accuracy for extrusion widths around 0.5 mm). If a micrometer is unavailable, consider [alternative calibration methods](https://ellis3dp.com/Print-Tuning-Guide/articles/extrusion_multiplier.html), noting potential impacts on precision.

## Measuring Tips
Measure 5 times, taking care to remove the calipers each time. Discard and re-measure any outliers, take the [median](https://en.wikipedia.org/wiki/Median) of the measured values as the value to enter into the spreadsheet.

Note that the [XY Reference Dimensions.stl](stl/XY%20Reference%20Dimensions.stl) model has "tabs" that allow you to easily align the jaws of the calipers by pressing the tab against the neck of the calipers, as shown below for repeatable measurements:

![tab alignment](images/shrinkage-square-tab-alignment.jpg).

### When Should You Re-Do the Calibration?
- Initiating a new filament profile.
- Any change to the extruder's `rotation_distance`.
- Adjustments to nozzle temperature settings due to their effect on material shrinkage.

## Calibration Procedure 
This procedure unfolds in two critical steps: First, we determine the material's shrinkage factor to scale the model pre-printing. Then, we calculate the extrusion multiplier, which hinges on the established shrinkage factor.

### Step 1: Material Shrinkage Calibration
1. Add a new filament entry in the calibration spreadsheet.
2. Print [XY Reference Dimensions.stl](stl/XY%20Reference%20Dimensions.stl) with the following settings:
    * Solid beams (high perimeter count).
    * External perimeters are printed first.
    * Layer height of 0.1 mm.
    * Reduced extrusion multiplier (by 5%).
    * Print speed: 60 mm/s, Acceleration: 800 mm/s^2.
3. Allow the model to cool on the build plate to room temperature.
4. Mark the X and Y axes.
5. Measure the 150 mm and 5 mm reference dimensions and record them in the spreadsheet. Refer to [Measuring Tips](#measuring-tips).

![Reference Dimensions](images/shrinkage-square.png)

Input the calculated scale factors into your slicer or keep them noted for future prints. If you're using  PrusaSlicer, consider upvoting the request for this feature [PrusaSlicer issue#4475](https://github.com/prusa3d/PrusaSlicer/issues/4475).

Should the calibration yield notably inconsistent X and Y scale factors, revisit the steps to identify any discrepancies.

### Step 2: Extrusion Multiplier
1. Input your filament's current extrusion multiplier into the spreadsheet.
1. Prepare the [Extrusion Multiplier.stl](stl/Extrusion%20Multiplier.stl) object for printing with these settings:
    * Slightly increased pressure advance to avoid corner bulge.
    * Wide extrusion width (150% of the nozzle diameter recommended).
    * Thin layers, preferably 0.1 mm.
    * Vase mode enabled.
    * A slow print speed of 60 mm/s and 800 mm/s^2 to reduce impact of pressure build up in the nozzle.
    * A brim for stability and gradual cooling post-print to prevent warping.
1. Post-print, let cool completely to room temperature, examine the part for warping or extrusion anomalies.
1. Measure once above each notches (8 total), as shown in ![notches](images/extrusion%20multiplier%20cube.png), and log these in the spreadsheet.
1. Update your slicer's material profile with the new extrusion multiplier from the spreadsheet.

### Step 3: Validate
1. Re-slice the [XY Reference Dimensions](stl/XY%20Reference%20Dimensions.stl) model, this time:
   * Scaling the model by the scale factor from the spreadsheet.
1. Print and measure again as in [Material Shrinkage Calibration](#step-1-material-shrinkage-calibration) but use the extrusion multiplier from the spread sheet instead.
1. The reference dimensions should be within 0.02 mm of the true values.

If the reference dimensions are off by a to you unacceptable amount, revisit the measurements and if that doesn't help, then restart the process from [step 1](#step-1-material-shrinkage-calibration).


## After Calibration
Congratulations, your primary material parameters are now calibrated and your parts are hopefully accurate!

Next steps involve tuning secondary printing parameters such as perimeter overlap, bridging speeds, elephants foot compensation, etc. 