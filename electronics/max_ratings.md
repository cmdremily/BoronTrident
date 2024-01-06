# Maximum Ratings for Boron Trident

All limits specified here are valid IF AND ONLY IF the printer is built as specced without and substitutions.

# Thermal Limits
## Print Bed
The [magnetic sheet](https://fermio.xyz/schallenkammer-magnetsysteme-gmbh/schallenkammer-graviflex-magnetic-sheet-200-360-x-360-mm/) used has a maximum operating temperature of 120°C and up to 200°C temporarily (whatever that means). The [build plate](https://fermio.xyz/fermio-labs-gmbh/voron-build-plate-350-x-350-mm/) is just a stress relieved aluminium sheet and were nowhere near temperatures where it would have issues. The [heater mat](https://fermio.xyz/keenovo-international-group-limited/keenovo-silicone-heatmat-340-x-340-mm-230-v-ac-500-w/) has a 150°C thermal fuse which we should avoid tripping even though it's self resetting. Note that the precision of this fuse is unknown and they are typically quite approximate, we de-rate by 10°K.

With the above the following maximum limits should be observed:
   * Max print temperature: 120°C.
   * Klipper shutdown temperature: 140°C.
   * Max short duration: 130°C
        * E.g. when heat soaking the chamber.