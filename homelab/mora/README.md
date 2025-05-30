# mora

mora is my custom homelab management tool. the idea started as i want to
describe my services using a docker-compose style configuration but deploy it to
k8s because i dont like writing k8s manifests. it has ballooned to also
including a management service and micromanagers for individual services, but it
is what it is. the architecture note goes into that more.

another aspect of it is the language itself. i decided to build its own language
because im a masochist. i really actually just wanna be able to practice my
language development and design stuff

## names

mora is made up of a few different components

1. **preflight** - this is the cli tool that reads the configuration in and
   sends it to the cluster
1. **runway** - this is the service running in the cluster that manages
   deployments and all that
1. **wingmen** - these are sidecar services that run alongside third-party
   services to glue everything together
