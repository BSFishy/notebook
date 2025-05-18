# homelab architecture

version 1 of the homelab is built on top of docker swarm. this sucks. it seems
to me that docker swarm is a mode built into the docker engine to try and
horizontally scale a docker setup. so like, they built docker, were like "oh, we
should be able to scale this for more enterprise setups", built docker swarm,
realized it was way too constrained, k8s came along, and development effort
moved to k8s. so swarm mode feels pretty terrible to use and some features that
i want seem completely unimplemented. for example, hardware acceleration in
jellyfin is pretty terrible.

version 2 of the homelab will be built on k8s, using a custom tool called mora.
mora will consist of a few parts. first will be the orchestrator, which will
take a configuration of docker-compose inspired files and generate a structure
for the homelab based on configurations. next, the configuration will be send to
the manager, which will be responsible for setting up, configuring, and managing
pods. each set of pods will also have a micromanager, which will handle
pod-specific configuration and lifecycle management.

this setup should provide me with a very scalable and decentralized (except for
the main manager processes) system, built on top of the power of k8s. it should
provide me with exactly the functionality that i want and need out of my homelab
and generally get out of my way when i want to spin up new services, manage
existing ones, or spin out a new network.

a major thing that i want to make sure the homelab is good at is spinning out
new networks. i am a programmer, not a systems administrator or whatever. i do
not like configuring things. the setup for the network should be as automated as
possible. mora takes care of much of this and ensure the system should be
scalable to the extent that i need it to be, in terms of auto-configuration and
all that.

## orchestrator

the mora orchestrator is a tool that is distributed as a binary. currently, i am
writing this in zig, so it should be distributed as a statically-linked native
binary. it will read in a mora project configuration, which should be a
configuration that is generic, in that i can upload my configuration to github
and anyone else can take it and spin out their own network, with exactly my
configuration, using their own domains, users, etc.

this will be facilitated by a custom configuration language. i mainly want to
use a custom language because i want practice building languages. literally
that's it. it doesnt give me any additional functionality or provide me with
anything that i would otherwise be unable to get somewhere else. i just want to
build a language.

however, having a custom language gives me a few benefits. i can make sure the
things that i think are important are first-class features in the language. for
example, i want to be able to generate passwords, api keys, ssl keys, etc.
without needing to jump through a million hoops. in v1, doing all of that was a
huge pain in the ass. in v2, i could make it a feature of the language, which
would automatically generate k8s secrets or whatever, which would be completely
opaque to me as the user of mora. it is entirely taken care of for me behind the
scenes.

a big part of this is dynamic configuration. i should be able to setup generic
things like "this should be a subdomain for this service". then, when setting up
the project, i go through some basic configuration, like "use this base domain",
so that anyone can setup the same setup that i have, but using their own
domains. that should be the case for everything that might generically be
configurable like that.

after the basic configuration, the orchestrator will read through the
configuration files, and generate a manifest of sorts. this will most likely be
some sort of json structure or something similar that can be sent to the mora
manager in the network. the mora orchestrator should be able to initialize a new
k8s network from scratch, so it will be pointed to a k8s network and if the
manager is not running, it will start it then send the manifest to the manager.

## manager

the next step in the process is the mora manager. this'll just be a service
running in the network, preferably horizontally scalably. its main function will
be receiving manifests and applying them to the network. effectively, just
receiving services, volumes, secrets, etc then deploying them to the network and
wiring them up as needed.

it should be mostly straight forward, except for dynamic configurations.
something that i've realized is that most services dont really like to be
provisioned. maybe i need to put an api key in a configuration file, or the
service expects to be configured through a wizard. regardless, it often kinda
hurts to try and pre-provision everything, which i've made clear is kinda a big
goal of this project.

however, i've crafted a solution (as i mentioned, im a terry davis-tier programmer).
the manager will host a web ui. this ui will give me some tools to perform basic
and common functions across the network. but it will also give me a setup
wizard. this will be pretty generic, in that any service can register something
to be configured, and it will be inserted as a step in the wizard. the wizard
will automatically determine which order services need to be deployed and
configured in and present the proper steps to the admin as needed.

for example, a step might be to setup the ldap server. this will include
creating an admin user, so inputting personal details and a password. but it
will also include, for example, configuring janitorr. janitorr requires api keys
from sonarr, radarr, jellyfin, jellyseerr, etc. so this will take you through
the setups for all of those services in order, then present a page that's
basically like "okay, click this link, create an api key, then insert it into
this text box". it will then take that api key, insert it into the configuration
file for janitorr, then deploy janitorr.

so the big thing here is that i have a completely guided and straightforward
path to setting up my network. it is centralized and takes care of all of the
tedious stuff of configuration, like putting all the api keys in a configuration
file. it also handles deploying the services in the order they need to be
deployed in.

a big piece of this is also that it will automatically figure out changes. so,
for example, if i deploy a new service that needs to sit in between the
configuration of 2 already deployed services, it will tell me that i need to
navigate to the setup wizard, prompt me for the new information that it needs to
configure the new service, then will use existing configuration values from the
previous setup to reconfigure and deploy the next service.

one might ask "well, if the manager is meant to be so generic, how will it be
able to configure all of these service-specific details?" uhm dont you think
i've already thought of that by now?

## micromanagers

micromanagers are special managers that wire up individual services to the mora
manager. it is effectively a sidecar service that communicates with the manager
to help with lifecycle events, configuration, and general tasks.

at its core, a micromanager is a rest api that the mora manager can connect to.
the mora manager will also have a rest api that micromanagers can connect to for
2-way communications. i think it's important that these are restful, because i
really really don't think i need socketed communications, but a micromanager
should still be able to query the mora manager for information and tell it to do
stuff when it is told to do stuff.

this will mainly be facilitated through a framework. a lot of it will most
likely be boilerplate, so i'll build out a small framework to make micromanagers
easy. written in go, it should be pretty simple to do things like "when the
manager wants to deploy this service, tell it that it needs to get this
information from the user" and then "when i get this information, spin out this
and that k8s resource for the deployment".

i can think of a few little scenarios where i might want generic or shared
information across the network, like api keys, so unsure of how much of this
sort of stuff will live in the micromanagers, but i do know that things like
configuring the ldap server will live there.

so for things like configuring the ldap server, i'll definitely need to have it
say something like "once the service has started, prompt for these pieces of
information in the setup", then use that information to bootstrap the ldap
information with the admin user, and a few groups, and stuff like that. by using
micromanagers, i can keep the implementation of that logic colocated with the
definition of the service itself, and through using a little framework, i can
keep the actual implementation of the micromanager to just the service-specific
logic.

## status

current stage: v1 implemented

i have the v1 currently running, and it seems to work decently well. i've only
really had issues 1 time, apart from the annoying jellyfin hardware acceleration
stuff.

for v2, i have started implementing the mora orchestrator, although i'm
considering restarting just to make sure the foundations are well done.
