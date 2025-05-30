# mora runway

the mora runway is a service that runs in the cluster and manages deployments.
it is where most of the implementation and logic will live methinks.

runway will receive a configuration from preflight. this is from an already
authenticated user in the runway system, to deploy a configuration to an
environment.

i want runway to have concepts of users. this doesnt add much i think, just some
user management tools and authentication. but it will allow me to set up a
cluster where i know people and can tell them they can use my cluster and we can
coexist on the same hardware, on the same k8s cluster with no issue. this should
not be difficult at all, just properly managing k8s namespaces.

additionally, each use will own environments. these are just different
deployment spaces so that i can deploy a staging and production instance on the
same hardware lmao.

each environment will have deployments. these are basically a tuple of
configuration + state + data. so, of course, if the deployment requires
configuration, i can incrementally configure it through the ui, as the
deployment progresses. this will be stored persistently so that in the event
that runway restarts or something like that or something, no progress will be
lost.

so this means that a little postgres database will need to be deployed alongside
runway to handle this across multiple runway instances.

the initial setup of runway will require setting up an initial admin user and
maybe include things like setting up dns and reverse proxy. we'll see about
that. but once that's done, preflight can be used to authenticate and deploy the
first environment. when that happens, a url will be provided to monitor the
deployment and configure as necessary.

what's super cool here is that runway just exposes an http server. any way i can
interact with an http server, i can use runway. so that means that for other
people, i can say, "yeah just connect to this public url" and it will work.
additionally, they can just go to that url to monitor and configure deployments.

## performing a deployment

so this is the main thingy. gotta figure out how to properly structure this.
luckily i've already figured it out. when we get a configuration from preflight,
we parse it into data structures. we then create a dependency graph based on
service dependencies (this means absolutely no cyclical dependencies, but i dont
think i was expecting to support those anyway). we then flatten that dependency
graph into a series of steps, ie services to deploy in order. we then go through
the list, see if this service requires any configuration, if so present it in
the webui, once we have all the info we need, deploy it. then just go through
that process for every service/resource in the list.

i believe storing the deployment at the step of having an ordered list of
services to deploy will be where we put it in the database. after that, the
deployment is considered "created" and the http request returns with info on
where to monitor it. once the deployment is started in the webui, it is started
and so on.

so one thing will be handling errors gracefully. it should detect an error and
mark the deployment as errored and be done with it. one interesting thing is
that it may be possible to roll back to a previous deployment. this MUST be done
with an extreme amount of caution, because configurations and versions may
change between deployments, so applications may break if they are rolled back.
in this case, it may make sense to back up volumes in the event of a deployment
to have a "safe rollback state" type of thing. we'll see about that. in this
case, a backup would be taken when a deployment is started. if the deployment
fails, i believe i would restore the backup volumes then deploy the previous
successful deployment. again, this has to be taken with extreme care, so we'll
see.

but yeah, we store configuration values input as part of the process that make
sense (not like api keys and stuff, those need to be stored as k8s secrets but
we can store the secret names and whatnot) for use in future steps. then it's
just running through what needs to be configured and actually doing it.
