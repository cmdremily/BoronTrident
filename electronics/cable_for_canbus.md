# A Primer on CAN Bus Cables for Motion Applications
I'd like to lead with the following quote I picked up somewhere ages ago:
> It's not enough to have something that works, you need something that works even if your neighbour starts vacuuming...  --Someone smarter than me

Getting reliabiliy out of a CAN bus system under motion requires careful selection of the cable and connectors both for electrical properties and mechanical properties. A poor cable selection can result in a system that mostly works, but has occasional failures at inopportune times, or that simply fails through mechanical wear.

## Mechanical Requirements
The main mechanical requirements are:

Category | Requirement | Comment
-|-|-
Bend radius and speed | Bend radius depends on the how the umbilical is supported, but 5 cm should be sufficient. Speed should be higher than maximum print-head speed. | Maximum bendradius typically when mounts-to-mount distance is at minimum.
Torsion | 90° over umbilical length from mount-to-mount  | This torsion occurs when moving diagonally.
Temperature | Maximum chamber temperature + 20° C for cable heating | This mainly affects what jacket materials are viable.
Oil/Chemical resistance | Weak/No requirement | There should be no oil or acid contact on the cable.

## Electrical Requirements
The main electrical requirements are:

Category | Requirement | Comment
-|-|-
Number of conductors | At least 4. Need 2 for signal and 2 for power | N/A.
Type of conductors | Twisted Pair at least for signal conductors. | Twisted bundle also acceptable if other requirements are met.
Power conductors cross-section | Depends on your voltage and toolhead, for 24 V and 4 A (96 W) you should stay above 0.5mm². | Figure out how much current you need at the toolhead, and then use an online calculator like [this](https://www.inchcalculator.com/voltage-drop-calculator/) to calculate a minimum cable thickness at 3% voltage drop.
Characteristic Impedance | As close 120 Ohm as possible | See below.

### What's Characteristic Impedance and why do I want it?
It's a rather complicated topic, but in essence: In a high speed signalling applications (above 30-ish KHz), like a CAN bus, you start to run into an issue called "reflection". Reflections degrade the signal quality, and depending on the severity can cause the CAN bus to occasionally have to retransmit packets, disconnect or plain refuse to work. We try very hard to reduce reflections to guarantee reliable operation.

Reflections are avoided if every part of the signal path between the sender and the receiver has the same characteristic impedance (or close enough), and by "terminating" each end of the "line" or bus with a resistor that has the same value as the characteristic impedance. We call this process impedance matching, if the impedance matching is poor, then there's an "impedance mismatch".

Typically communication protocols like USB, Ethernet, and CAN bus defines an impedance value that should be kept on every part of the signal path and at the ends, the termination. For CAN bus, the standard specifies this value as 120 Ohm.

These communication protocols have a built in margin of error and can tolerate some reflections from small mismatches and other interference, the remaining signal margin is often called "signal headroom". The less headroom your signal has, the more likely it is to have a transmission error, and the less robust it is against other interference in your surroundings.

For the above reasons, it's very important that the cable that is used for a CAN bus has a characteristic impedance of 120 Ohm, or as close as possible. This means that using whatever pair of twisted cables you have laying around without knowing what impedance it has, will result in an unknown headroom for your CAN bus signal, if it works at all. Essentially, you do not know how close the signal is to being unusable.

### What About Twisted Pair?
Twisted pair cable has two important properties for signal transmission:

1. Twisting the wire together icreases the mutual inductance and capacitance of the pair, contributing to the characteristic impedance of the cable.
2. Differential signalling over a twisted pair makes the signal pretty much immune to common mode noise. I.e. noise that affects both conductors in the pair evenly, or in a "common mode". Basically if you send the signal on the positive wire as  `p(t) = x(t)`, and `n(t)=-x(t)` on the negative wire, then both are affected by the same noise `e(t)`, then the received signals become `p'(t)=x(t) + e(t)` and `n'(t)=-x(t) + e(t)`. The receiver takes the difference of both the received signals as `r(t)=p'(t) - n'(t)=x(t) + e(t) - (-x(t) + e(t)) = 2*x(t)`, the noise signal is completely cancelled out.

### What About shielding?
Because of the noise immunity of a twisted pair cable, it doesn't need to have shielding to maintain signal integrity, as shown above. The shielding on a twisted pair cable is to protect other, non-twisted pairs from the EMI caused by the twisted pair itself, or to meet EMI regulation.

## A word on connectors
The most important property of the connection is that it's secure during movement and that it can carry the required current. JST XH is rated for up to 3 A and Molex MicroFit 3.0 is rated for up to 8.5 A.

The second most important property is if it's secure under motion. Molex MicroFit 3.0 has a clip latch that makes it so that the cable cannot unseat itself without the latch being opened, making them secure. JST XH is a rather strong connection but can dislocate if enough force is applied (heavy and long cables are a bad idea). As for Dupont connectors, don't. 

## A word on RX/TX errors
Can the RX/TX errors reported in software be trusted? Maybe. Probably.

It depends on the nature of the error. It seems like at least the EBB36 might just lock up and stop responding all together until it's power cycled. To the host this would be more similar to "host unavailable" instead of "tx/rx error" so it might not necessarily show up in "tx/rx errors".

However, if the other end didn't lock up and asked the sender for a re-transmission of a garbled packet, then it's up to the implementation of the transmitter and receiver whether they report this to the OS or firmware, or just handle it quietly and transparently. Without knowing the implementation, we cannot say for sure.

We know that Klipper will keep track of the stats if they are available to it.

## Cable Recommendations

With all of this verbage can you recommend some cables?

Vendor | Product | C. Impedance (@1MHz) | Max Recommended Power (3m @ 24V, 3% Vdrop) | Bend Radius (60°C) | Torsion | Max Temp | Comment 
-|-|-|-|-|-|-|-
igus | [CF9.05.04](https://www.igus.ie/product/CF9?artnr=CF9.05.04) | Measured as 100 Ohm | 96 W | 42mm @ 10 m/s [^1] | ±90°, with 1m cable length | 100 °C | See the [investigation](CF9.md).
igus | [CF77.UL.05.04.D](https://www.igus.eu/product/CF77_UL_D?artnr=CF77.UL.05.04.D) | Calculated as 100 Ohm (?) | 96 W | 51mm @ 10 m/s [^2] |  ±180°, with 1m cable length | 80 °C | PUR jacket can be though to work with.
igus | [CF113.018.D](https://www.igus.eu/product/CF113_D?artnr=CF113.018.D) | unknown | 96 W | 61.75 mm @ 10 m/s[^2] | None |  80 °C | Not recommended for long term umbilical use due to no torsion rating. One extra twisted pair.
Various | [CAT5](https://en.wikipedia.org/wiki/Category_5_cable) or better | By design 100±15 Ohm | 28 W (24 AWG) | unknown | unknown | 60 °C | Suitable for short term use up to 60 °C chamber temp (no extra margin needed for self heating as power is not carried in jacket), with power provided separately.

[^1]: Based on igus chain stress capacity of 7 and life time table, 12.5 million double strokes is guaranteed.

[^2]: Based on igus chain stress capacity of 5 and life time table, 10 million double strokes is guaranteed.