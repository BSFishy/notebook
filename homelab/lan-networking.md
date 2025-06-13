# lan networking

okay so to get lan networking, im gonna need to get a few things working
together. but its not all that complicated.

effectively, what i want is a set of opaque nodes. all of these nodes expose the
same ports for the most part and can be connected to in the same way. these are
primarily ports 53, 80, and 443. this will allow me to do dns, https, and http
(we'll need to see, probably not, only for basic testing). once the request
makes its way onto the cluster, it will automatically be routed to the correct
node to be handled.

traefik running as a daemonset is the primary ingress point. it will run on all
nodes and expose ports 80 and 443. it will then route the request to the correct
service. it will also handle tls. it will just be listening on the cluster for
new ingress resources and automatically update when new ones show up.

next up is dns. similarly, there will be coredns, which will provide dns. its
super flexible with plugins and all that so i should be able to lock it down
pretty tight. i dont know if it can directly get dns records from k8s, but
that's where the next service comes in

external-dns is a service that listens for ingress resources and updates dns
servers accordingly. this is how cloudflare will automatically be updated to
support these services. i believe i should be able to make it write the records
as cname to the tunnel, which will make everything just kinda work.

we also need cert-manager, which is a service that will automatically provision
ssl certs for the domains. similar to the rest, it listens for ingress and will
provision as needed. traefik will pick up certs and use them automatically.

for any other services that need egress or ingress, i will just use host ports.
this is, for example, jellyfin service discovery and home assistant discovery.
that way, i get all the horizontal scaling and all that and dont even need to
think of nodes or anything as separate. free load balancing (not actually free)
and all that.

additionally, i should have a way to say that a service SHOULD NOT be served
over lan, only over wan. this is primarily only for gotify, which i dont want to
have to worry about roaming off of the lan causing disconnections and missing
out of notifications because i have to deal with android not respecting dns
ttls.

after all that, i should have an incredible, scalable system that enables me to
connect directly to everything over lan and still connect to it over wan.
although, of course, it will overall be wayyyy faster over lan and all that.
lan-first routing
