# dumb home

my vision for my home is a smart home that feels like a dumb home. i should be
able to control my lights with physical, analog switches but i should also be
able to control them with software. that sort of vibe where it _is_ smart, but
it doesnt look the part. its just all the little conveniences that technology
allows without any of the flashy bullshit.

a big part of this will be home assistant and mqtt. i know what ha is but mqtt
is a pub/sub system for smart devices. it allows for lightweight communication
of intention across a network, driven by something like home assistant.

another bit is esphome. that is a cool little piece of software that flashes an
esp32 or esp8266 with some custom firmware to make it a smart device, connect it
to wifi and home assitant, enables ota updates, and more, all configured using
yaml. i'll have to see how good it is but it sounds pretty incredible,
especially for like little sensors or basic controllers.

the last thing will be zigbee. zigbee is a protocol for smart devices that is
parallel to wifi/bluetooth that should be more low power, have less
interference, and overall work better that putting everything on wifi. it feels
like zigbee should be the go-to if possible. it just requires some usb connector
to do zigbee2mqtt then we're good to go.

i envision having little things like maybe little analog control panels. solid
wood with brass knobs and switches to control my home things. like a knob for
music volume or buttons to run some automation or whatever else. just a little
board inside that connects to ha and then we're basically good to go.

most likely, i will building a lot of my own electronics for this kind of smart
dumb home. chatgpt recommended some marketplaces:

- digikey - massive inventory, industrial-grade, fast us shipping
- mouser - great for semiconductors, sensors, connectors, also us-based
- adafruit - beginner-friendly, curated parts, great docs and tutorials
- sparkfun - similar to adafruit, with more engineering-focused kits
- tindi - indie creators selling weird/cool boards and modules

and then some like music synth module marketplace recommendations (these also
sell individual parts):

- love my switches - high-end switches, knobs, footswitches, toggles (super
  currated)
- thonk / modular addict - synth module parts (super tactile, aesthetic stuff)
- mcmaster-carr - pro hardware, hinges, panels, etc.

overall, i see lots of raspberry pis, esp32s, and zigbee things showing up
around my home.
