# better smart lighting

i dont want smart lighting that just kinda :sparkle: turns on :sparkle: at
random times. i want it to be way more smart and feel a bit better than just on
off.

first up is motion sensing. millimeter wave radar is a phenomenal technology
that can detect movements as small as even heartbeat even through materials like
clothing. ideal for proper, good presence detection in a space. this is what i
want to use for knowing if a space is occupied.

i want to bring in a lot of information to know _if_ a light should be on, and
if so, _how much_. for example, at night, i would like the light to fade on to a
very low level if presence is detected. all of the changes to light level should
be gradual over a second or a few seconds. and it should generally be that.
based on time, current ambient light level in the space, and presence.
additionally, i should be able to override this automatic behavior and toggle a
light on or off directly with a switch or something.

i can find a mmwave sensor and hook it up with esphome. that should be pretty
cool for some presence detection. philips hue lights are all zigbee so that
should be pretty good for lighting. i'll need to see about ambient light sensors
but im sure that will just be another esphome thing.

additionally, lighting should be for a room. i want to flip a switch and the
room turns on. i dont want to have to turn on each light individually.

all managed through home assistant, zigbee, mqtt, etc.
