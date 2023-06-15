# Electronics Do Nots
Below is a non-exhaustive list of things that will get you into various kinds of trouble.

* Working on a printer that's has a power coord connected
    * Don't do it, I don't care how careful you are.
* Not using the correct colours for wires
    * Use EU colours for AC as they don't have overlap with DC colours. Using standard colours makes it easier for others to help you and to troubleshoot any issues, and keep you safe.
    * AC Live: Brown, The colour of your pants if you touch it.
    * AC Neutral: Blue, The colour of the sky when you're relaxing.
    * AC Protective Earth (PE): Green and Yellow, the colour of daisies and grass growing on earth.
    * DC Ground: Black, always, don't use black for anything else.
    * DC Power: Red or Yellow (but only for 12V). Don't use these colours for anything else.
* Confusing "Ground", "Protective Earth" and "Neutral"
    * None of these should ever be connected together, magic smoke will escape or a fuse will pop, guaranteed.
    * Ground refers to 0 V DC, sometimes denoted V-.
    * Protective Earth refers to "GFCI" 
    * Neutral is the return path for AC current (nominally, in many places you don't know which wire is neutral or live as the plug is reversible)
* Using a multimeter on mains voltage
    * If you're reading this list, you probably can't do this safely.
* Order electrical components of Amazon/AliExpress
    * Many (unofficial) sellers don't know what they're selling. It's easy to end up with counterfeit or wrong parts. Prefer to order from reputable electronics distributors like DigiKey, Mouser and RS.
    * If it doesn't come with a datasheet, it's sus.
* Not checking for shortcircuits before powering on
    * A short circuit between any combination of Ground, 5V, 12V, 24V, Live, Neutral or PE will either release the magic smoke or cause a breaker to pop, or cause a fire later.
    * Before powering on, use a multimeter in resistance mode (100K range if applicable), between each pair of the above, make sure you see at least 10 KOhm.
* Over loading a power supply or power rail
    * You need to calculate how much power you might pull at peak from each power supply, and make sure you do not exceed the maximum power/current rating.
    * The above also applies to 5V, 12V other voltage supplies on boards like the Octopus.
    * If you don't know how, ask for help.
    * Overloading or consistently red-lining a power supply or power rail can cause it to break.
* Soldering wires
    * Always use connectors and crimped wires.
    * Yes, if you know what you're doing, there are some specific situations where you can solder a wire, but if you know that, you're not reading this.
* Getting ground and power (5V/12V/24V) swapped
    * Magic smoke, instantly.
* Using a multimeter in resistance mode on an energized circuit
    * Your measurements will be incorrect garbage, don't do it.
* Not strain relieving cables that move
    * They will break, and possibly cause one or more of the above things to also happen. Roll a D20.
* Assuming that something will work or be installed a certain way
    * There are more ways to destroy your equipment than to hook it up correctly. Don't assume, read the manual. If it doesn't come with a manual, don't buy it.
    