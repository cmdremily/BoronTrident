# Material Shrinkage and Extrusion Multiplier Tuning
The final dimensions in the XY plane of a printed part are affected primarily by the XY motion system, the material shrinkage and extrusion width. We've already calibrated the XY motion in [XY Differential Calibration](xy-differential-calibration.md), here we will treat the other two sources of error to hopefully produce dimensionally accurate parts.

## Why This Calibration?
Conventional material shrinkage calibration methods directly derive the scale factors from measuring a reference object, inadvertently incorporating errors like extrusion width and measurement biases. Such approaches, conflate material shrinkage with unrelated additive errors, leading to imprecise results.

Our method uses differential measurements, akin to zc isolating material shrinkage by cancelling out additive errors. This results in an accurate scale factor for shrinkage alone.

For extrusion multiplier calibration, traditional techniques measure extruded width post-shrinkage. This causes more material to be extruded than the slicer requested.

We compensate the measured extrusion width by the above shrinkage factor to make sure that the actually extruded amount of material is as intended.

The details and proofs are in the [paper](shrinkage-and-multiplier-calibration.pdf).

### Typical Results
After carefully following first the [XY Differential Calibration](xy-differential-calibration.md) procedure and then the material and extrusion width calibration as explained here. The validation step below produced an actual length of 150.03 mm and 5.00 mm on the X axis and 150.01 mm and 4.96 mm on the Y axis of the 150 mm and 50 mm nominal distances respectively. 

After XY calibration but prior to shrinkage/extrusion multiplier calibration, the part measured: 148.83 mm and 4.92 mm on X, and 148.74 mm and 4.91 mm on Y respectively. The material used was black Prusament PC Blend which has an estimated shrinkage factor of approximately 0.992, or 0.8% determined from this calibration procedure. For reference pistachio coloured Prusament PETG has a shrinkage factor of about 0.4%, likewise determined experimentally from this procedure.

From these admittedly few data points we can infer an accuracy of ±50 um over 150mm or 0.03% error in this example.

The fact that the larger Y dimension is very close to true while the smaller dimension has a larger absolute error seems to indicate that either the skew correction or the XY Calibration can be improved on my machine. As such an error cannot come from the part scaling alone.

N.B.: This material calibration can compensate slightly for errors in XY calibration as it's performed "on-top" of any errors introduced from that. In other words the material shrinkage factor has any inaccuracies from the previous calibration "baked in", and should not be taken as absolute truth. However shrinkage compensation and extrusion multiplier should transfer reasonably well between printers that are calibrated equally well for XY motion.

N.B. 2: Skipping XY Calibration and only doing skew correction before doing this calibration can produce parts that are dimensionally accurate to some degree. However as the toolhead speed can be inconsistent between A- and B- axis in this case, extrusion width may vary and cause inconsistencies.

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
- For every new filament.
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

Input the calculated scale factors into your slicer or keep the spreadsheet around for future prints. If you're using  PrusaSlicer, consider upvoting the request for this feature [PrusaSlicer issue#4475](https://github.com/prusa3d/PrusaSlicer/issues/4475).

Should the calibration yield notably inconsistent X and Y scale factors, revisit the steps to identify any discrepancies.

### Step 2: Extrusion Multiplier
1. Enter the current extrusion multiplier into the spreadsheet.
2. Print the [Extrusion Multiplier.stl](stl/Extrusion%20Multiplier.stl) with these settings:
    * Slightly increased pressure advance.
    * Wide extrusion width (150% nozzle diameter).
    * Thin layers (0.1 mm).
    * Vase mode.
    * Slow print speed and acceleration.
    * A brim to prevent warping.
3. Using a marker, number the notches as in the spreadsheet as an aid. It's easy to lose track.
4. Let the part cool, inspect for warping, then measure the wall thickness with a micrometer above the 8 notches, noting the values in the spreadsheet.

![Notches](images/extrusion%20multiplier%20cube.png)

Update your slicer with the new extrusion multiplier from the spreadsheet.

### Step 3: Validate
1. Re-slice the [XY Reference Dimensions](stl/XY%20Reference%20Dimensions.stl) using the scale factor and new extrusion multiplier.
2. Print and measure again. Aim for reference dimensions within 0.02 mm of true values.

If dimensions are not satisfactory, review your technique and measurements, and if necessary, restart from step 1.

## After Calibration
Congratulations, your primary material parameters are now calibrated and your parts are hopefully accurate!

Next steps involve tuning secondary printing parameters such as perimeter overlap, bridging speeds, elephants foot compensation, etc. 