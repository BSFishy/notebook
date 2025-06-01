# whole home audio

this is a super exciting idea. so if im not mistaken, sonos has some
functionality or feature where it will play audio synchronized throughout your
entire home. i want that but fully diy and hooked into my homelab stuff. sounds
crazy right? of course it doesnt that's how technology should be; good.

so the setup that i'm looking at is roughly made of 3 software pieces. snapcast,
mopidy, and iris.

snapcast is a server-client streaming system. it will connect a bunch of
snapcast clients to a central snapcast server and synchronize them all together
and send music to each client to play audio across the entire network (or only
parts of the network!) in sync. if that wasnt enough, it supports multi-source
audio, so it could stream music and play some home assistant tts or something
like that at the same time.

mopidy is a music server. it is the service i can connect to spotify or whatever
other music source i want and will play music from a central source. so mopidy
connects to snapcast and gives snapcast a stream of audio to play across the
clients.

iris is a web ui for mopidy. it will allow me to control what is playing from
mopidy from the web. thats pretty much it i think.

so all of these pieces wired together should give me a super nice system for
having speakers around my home so that i can hear music nice and clean no matter
where i am. it should also give me the opportunity to play any other audio i
want. i could wire snapcast with home assistant to play whatever audio i want
from there too. again, snapcast can stream audio from multiple sources, so i
should be able to play whatever audio i want across my entire home.

a fun little extension of this will be speakers. most likely, the setup will be
raspberry pis connected to a speaker, diy. it would be fun to also build the
housing for the speaker out of wood and have a nice cohesive set of speakers
that all sound nice and are synced with the rest of the home.

another nice thing would be running a spotifyd instance alongside mopidy. that
way i can connect with spotify connect and control the music playing from the
native spotify app. the added benefit would be that anyone visiting my home can
also just connect to the speaker in their spotify app and play whatever they
want.

with the better smart tv, this creates a little annoyance, because mopidy offers
a websocket api to get info about the current media playing but spotifyd doesnt
offer one out of the box. and i want the tv app to be able to support both
streams.

so the plan i have devised is to make use of mqtt to send pub/sub notifications
about what is currently playing. it will most likely entail sending events from
both mopidy and spotifyd (and anything else i may add at some point), collecting
those events and sending a unified event that is "now playing" and listen for
that event on the tv to know what is currently playing. that should give me the
opportunity to mess around with mqtt along with all of this stuff.
