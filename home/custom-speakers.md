# custom speakers

so i want to make custom speakers to hook up to my whole home audio system.
beautiful, elegant, unassumingly smart speakers. hard wood exterior, incredible
driver, powerful amp. a wonderful little package for a delightful experience.

this wont be too crazy. get a raspi, hook it up to a dac/amp, hook it up to a
driver, we're golden. build a housing around it. great. need to make sure the
housing is properly sealed and contains good damping, but other than that, it
should be a pretty great little speaker. best part is that if i ever dont want
it to be smart anymore or whatever i should be able to put a line in on the back
and use it as a regular speaker. wonderful.

chatgpt recommends drivers from dayton audio, peerless, tang band, faitalpro,
visaton, etc. and getting them from parts express or madisound. looks like they
both have a pretty good range of low to high end drivers and amps. ill most
likely want to start out with a full range driver and maybe split into a 2way
system for better richness.

for amps, chatgpt recommends class d amps because they're small and efficient.
it recommends TPA3116D2 boards (cheap, powerful, stable), HiFiBerry Amp2 (pi
hat, powers pi and amp from one source), MAX98357A (i2s for esp32, tiny but
decent for small setups). also gotta make sure it matches the drivers impedance
and can deliver enough wattage without clipping.

a full setup chatgpt recommends is

| part              | example             |
| ----------------- | ------------------- |
| full-range driver | dayton audio ps95-8 |
| class d amp       | tpa3116d2 board     |
| smart brain       | raspi 3/4           |
| dac               | usb dac or pi hat   |
| power             | 12v 5a psu          |

but i think ill use the hifiberry hat
