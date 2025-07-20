# networking

i want to plan out the networking of my homelab so that i dont completely mess
anything up. so here that is.

i want to break my network up into 4 vlans:

1. servers
1. personal stuff
1. guest network
1. iot stuff

pretty self explanatory, but i'll have one network for all of my servers and k8s
stuff, one network for _my_ computers, laptops, phone, tvs, etc. i'll have one
network for guests and one for sensors, cameras, speakers, etc.

all separated by vlans of course, and in different subnets. i think ill split it
like this:

- servers - `10.0.1.0/24`
- personal - `10.0.2.0/24`
- guest - `10.0.3.0/24`
- iot - `10.0.4.0/24`

and that should be pretty good i think. personal and guest should be pretty
simple, just regular dhcp networking stuff. iot similar. and servers no dhcp at
all. all static ips and specific ranges.

i want to split up the server vlan into a few ranges for different things:

- static assignment - `10.0.1.1-10.0.1.32`
- virtual ips - `10.0.1.33-10.0.1.223`
- well known services - `10.0.1.224-10.0.1.255`

so this should give me a decent address space to play around with stuff. i can
have 31 physical devices to assign static ips to, and 31 ips for well known
services. everything else is dedicated to ips that can be used for miscellaneous
things within the cluster.

well known services:

- `10.0.1.250` - dns service
- `10.0.1.251` - k8s_gateway dns service
- `10.0.1.252` - web ingress

so the reason i want to have two well known dns services is because i run
k8s_gateway as a separate service. this is just a dedicated coredns instance
that runs that plugin and that i forward from the original service to in order
to get those mappings. i want it to be well known so i dont mess anything up
within the cluster ips.

apart from that, i have a lot more room to add services and all that. super
cool. also, if i ever need more space, for whatever reason, i can just add
another address range to that vlan like `10.0.5.0/24` or something like that
with the same or maybe even different allocation.
