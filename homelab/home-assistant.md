# home assistant

so running home assistant in k8s is not impossible, but is gonna require some
additional setup to get all of the features working properly. running the ui and
all that isnt gonna be an issue, but what will be is all the networking stuff.

home assistant uses mdns and ssdp to find devices on the local network and
automatically set them up. this allows devices to find home assistant and home
assistant to find devices. additionally, bluetooth is useful to connect to
things like chromecast.

mdns and ssdp are the main problem children. they broadcast to some multicast
ips on the network, which k8s doesnt handle well out of the box. there are a few
solutions to this, such as running avahi-daemon, but ideally those packets can
just pass directly through to the containers.

[multus](https://github.com/k8snetworkplumbingwg/multus-cni) is a container
network interface plugin for k8s that allows attaching multiple interfaces to
containers. this means that i attach an interface to each container that just
communicates with the lan. it gets its own ip and can send and receive mdns,
ssdp, broadcast, etc packets over the network as if it were just connected
directly to the network.

using multus, i can have my services like home assistant or jellyfin just be
connected and not have to worry at all about making sure mdns or ssdp work
properly, they are just devices connected to the network and can see these
packets and interact with the network like any other device.

on potential problem might be how multus needs to be setup. it can be setup with
macvlan, ipvlan, or bridge networking. its sounding like bridge networking is
out of the question. so macvlan effectively just uses the same nic but bypasses
the host os ip stack so packets go directly to/from the container. so each
container gets its own mac address and connects to the network like that. ipvlan
however shares the same mac address. also, macvlan doesnt allow connecting to
the containers from the host (although i doubt i will need that anyway).

macvlan sounds like it will be the easiest solution that works, however
compatibility might be painful with the spectrum router. ill be getting unifi
hardware at some point, which will let me do a bunch of cool crazy stuff, but
until then, i need to work within the means that i am in. this means that i
should try macvlan first, but if that doesn't work, ill need to try ipvlan. if
that doesn't work, im pretty much sol. but ipvlan should have better
compatibility but it just might be a little more complicated to set up.

with that, bluetooth should be pretty simple. just need some node affinities and
passthrough to get the device and we should be good to go.
