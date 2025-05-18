# homelab

the homelab is a network i am running at my home. it hosts a number of services
and tools for me to get away from cloud stuff. a few benefits would be complete
ownership and control over my data, complete control over the services i use,
and a learning and tinkering space for messing around with stuff.

the main mission would be getting away from the cloud. i want a network that i
can be connected to no matter where i am that i can be confident will only ever
go down if i fuck something up (which is never because i am the greatest
programmer that has ever existed). this transitively means that even if the
wider internet ceases to exist, my day to day operations will be unaffected
because i should be able to access and use everything that i use day to day on
the local network.

all of this is to say that the primary functions of the homelab will not call
out to the internet except for auxiliary function. for example, i might call out
to the internet when upgrading container versions, but will never call out to
the internet for ai inferencing (unless i explicitly call out to an external
service, which will only happen for testing and comparisons).

even though the network will be built in a way that it is completely accessible
locally, it will also be accessible remotely. if i go on vacation, my workflows
should be completely unaffected. meaning i can go to the same urls, connect to
the same services, whatever, as if i was connected to the lan directly. this
will mostly be taken care of by cloudflare zero trust for now, although i am
interested in seeing what tailscale provides. i think i generally want to be
able to connect to the same urls without needing to connect over a vpn or
something similar, but we'll see.

additionally, it will serve as a playground for myself. i am constantly
improving my setup and making my workflows faster and more tailored towards
myself. the homelab will serve as a playground to test things out, work or
otherwise, for me to learn about new technology, and play around with fun ideas.
i am infatuated with scaling, so of course scalability will be a major theme of
the homelab.
