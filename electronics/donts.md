# Electronics Do Nots
Below is a non-exhaustive list of things that will get you into various kinds of trouble. The number doesn't indicate severity or relative importance, but exists solely so that specific items can be referred to easily in text.

## General
1. Working on a printer that has a power cord connected
    * Don't do it, I don't care how careful you are.
    * Turning off the mains switch does not count. Unplug it anyway. 
1. Assuming that something will work or be installed a certain way
    * There are many more ways to destroy your equipment than there are to hook it up correctly, double check everything. Don't assume, read the manual. If it doesn't come with a manual, don't buy it.
    * Even things that appear to "work" may later cause damage or kill you, especially mains wiring. Electrical history is littered with the bodies of smart people who made something "work" but in an unsafe way. 
    * Do not assume wires are coloured correctly or consistently. Always trace or probe power wires before making modifications or connecting a new device. 
1. Hot-plugging connectors that aren't designed for it
    * Only connectors that are designed for it (like USB, SATA etc) may be hot-plugged. These cables are designed so that ground and power make electrical contact before data pins do. If a data pin is high, and connects after ground, but before power, the chip can be destroyed.
1. Order electrical components of Amazon/AliExpress
    * Many (unofficial) sellers don't know what they're selling. It's easy to end up with counterfeit or wrong parts. Prefer to order from reputable electronics distributors like DigiKey, Mouser and RS.
    * If it doesn't come with a datasheet, it's sus. The datasheet should be available before you buy the part. 
    * Especially never order mains or safety critical parts from anything but a reputable supplier. 
1. Be aware of failure modes and their consequences. Vorons are designed to be as fail safe as possible, however, there are a few things to watch out for:
   * MOSFETs really like to fail short. These are devices that act as an electrical switch to deliver power to other components, notably, the hotend heater. This means if the heater controller MOSFET fails short, it cannot be commanded open and your heater will heat until it self destructs or power is removed. *Thermal runaway protection does not protect against this particular failure*. If this concerns you there is a pinned post in the Voron Discord electronics channel with instructions for how to make the system safer. 
   * Do not assume any over-current protection devices such as fuses or circuit breakers will necessarily protect you or your printer. They are much better than nothing but are not 100% reliable (nothing is). This is one of the reasons you should always check for shorts. Notably, fuses will not protect you from failures such as thermal runaway because those draw a normal amount of current.

## When wiring
1. Not checking for short circuits before powering on
    * A short circuit between any combination of Ground, 5V, 12V, 24V, Live, Neutral or PE will either release the magic smoke or cause a breaker to pop, or cause a fire later.
    * Before powering on, use a multimeter in resistance mode, between each pair of the above, make sure you see at a few hundred ohms
      * Pay attention to which resistance range is selected on your multimeter if applicable. OL means open line, i.e. more resistance than the range can mean. Some multimeters show "error" or "1  ." or some thing that's not a number to mean out of range.
      * Do not use continuity mode for this
1. Confusing "Ground", "Protective Earth" and "Neutral"
    * None of these should ever be connected together, magic smoke will escape and/or a fuse will pop, guaranteed.
    * Ground refers to 0 V DC, sometimes denoted V-. This is also called return. 
    * Protective Earth (PE), sometimes called safety ground, refers to the Green/Yellow striped, Green, or Bare (US) wire in your mains connection. The purpose of this wire is to protect you from electrical shock/electrocution in the event that the hot mains wire comes lose and energises some or all metallic parts of a device. It does so by connecting all metallic parts with a low resistance path to your fuse box, so if the hot connector touches any metal part, a large short circuit current will flow and trip a fuse in your fuse box.
    * Neutral is the return path for AC current (nominally; in many places you don't know which wire is neutral or live as the plug is reversible). This is also known confusingly as the grounded conductor. Despite this terminology, PE and Neutral are **not** interchangeable. 
1. Getting mains wires swapped
    * Magic smoke, breaker pop, fire, or all of the above, instantly.
1. Getting ground and power (5V/12V/24V) swapped
    * Magic smoke, instantly.
1. Getting any of the power rails (5V/12V/24V) swapped
    * Believe it or not, also instant magic smoke.
1. Soldering wires
    * Always use connectors and crimped wires.
    * Yes, if you know what you're doing, there are some specific situations where you can solder a wire, but if you know that, you're not reading this.
    * If you need to solder a wire to a pad (for example, when wiring LED tape) be sure to strain relieve it. Hot glue works well and is removable. 
1. Not strain relieving cables that move
    * They will break, and possibly cause one or more of the above things to also happen. Roll a D20.
    * Avoid zip ties for strain relief. They can put stress on wires and cause them to break if they are over-tightened. 
1. Not using the correct colours for wires
    * Use EU colours for AC as they don't have overlap with DC colours. Using standard colours makes it easier for others to help you and to troubleshoot any issues, and keep you safe.
    * AC Live: Brown, The colour of your pants if you touch it.
    * AC Neutral: Blue, The colour of the sky when you're relaxing.
    * AC Protective Earth (PE): Green and Yellow, the colour of daisies and grass growing on earth.
    * DC Ground: Black, always, don't use black for anything else.
    * DC Power: Red or another bright, warm, colour. Don't use these colours for anything else.
      * Consider using different colors for positive nets of different voltages. Stick to bright, warm colours. Alternatively use all red and tag with coloured electrical tape (bonus points if you label it)
    * Low Voltage Signal: Use colours that are not used for anything else in the system. Most low speed signalling protocols do not have colour standards so use what makes sense to you and be consistent. 
1. Overloading a power supply or power rail
    * You need to calculate how much power you might pull at peak from each power supply, and make sure you do not exceed the maximum power/current rating.
    * The above also applies to 5V, 12V other voltage supplies on boards like the Octopus.
    * If you don't know how, ask for help.
    * Overloading or consistently red-lining a power supply or power rail can cause it to break.

## When using the Multimeter
1. Using a multimeter on mains voltage
    * If you're reading this list, you probably can't do this safely.
1. Using a multimeter in resistance mode on an energized circuit
    * Your measurements will be incorrect garbage, don't do it.
1. Using a multimeter incorrectly
   * Multimeters all operate slightly differently but work on the same general principle
   * Make sure you are set to measure the correct property before you connect the probes. Especially make sure you are not set in current mode whem measuring voltage. This shorts the nets between your probe and will at best blow your meter's fuse. Some multimeters have a special plug for measuring higher currents. This is always connected to a short between that plug and COM, so it will cause a short when you probe even if the meter is set to voltage or resistance mode.
   * Use continuity only for checking if wires are in general connected, not to see if they conduct particularly well. This is best used when you cannot see the meter screen while probing and need an auditory cue instead. If you are unsure what this means, do not use continuity mode. 
   * If your meter does not auto-range, be sure to select the range that is appropriate for the value you expect to see. If you are unsure what range is correct, start at the highest and work your way down until you get a meaningful reading. 
   * If you need help with your particular meter, ask for help.
