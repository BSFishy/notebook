# a better smart tv

so i use my tv for a small number of things. mostly youtube, twitch, and music.
since im looking to add a whole home music system with snapcast, i want that on
my tv, and while we're at it, might as well add some other stuff like steamlink.

so to do this, i dont want to have to switch between a bunch of inputs. my fire
tv stick does not do what i want so gotta switch to something else. a raspi is
looking like the most likely solution. gives me a regular linux box that i can
do whatever on.

right now, kodi seems like the best option unfortunately. like fortunately, it
should give me everything i want like youtube, jellyfin, twitch, and snapcast.
but it looks ugly. other than that its great lmao. good remote support, i can
watch and do all the things i want, etc.

steamlink is a bit unique. it should be using the native steamlink app. that
should give the best support and all that so that i have the best experience.
however that means it needs to be started using a script which isnt a big issue,
just something i though should be called out.

snapcast is also an interesting one. it will run as just a background service
and play audio and all that whenever. that's fine. but i also would like an app
on the thing so that i can see the current song being played, skip to next, etc.
there isnt one for kodi yet so i'll need to make one. no big deal just gotta
figure all that out.

finally on to the ugliness. themes exist but apparently they all kinda suck. as
in they break frequently or are old or whatever. not a big deal, i want
something very specific anyway. i want startup to give me a grid of tiles with
all the apps that i want. then like youtube should give me a grid of videos to
watch, twitch should give a list of live streams, jellyfin should just be
jellyfin, etc.

this shouldnt be too hard but apparently the skin system in kodi is absolutely
abominable. lots of xml junk and whatever. so i say sethbling made cbscript cuz
command blocks were painful to use, ill build kodi skin script cuz kodi skins
are painful. or something along those lines.

but yeah the plan is to build some custom language that eventually compiles into
some skin for kodi so that i can build the skin quickly, efficiently, and maybe
provide a tool to the community for building better skins faster :shrug:

i _could_ do all that but also apparently kodi exposes some backend api. so what
i also could do is use a small, simple wm, start a custom application first,
tell the wm to focus the correct application, then it can call out to kodi or
steamlink.

this will give me full control over what it looks like and i can build a web ui
using tauri or something similar. so it should be pretty good. there are cec
libraries for rust, and it should offer me the best of both worlds i think. if i
build it well, i should be able to tell the wm pretty well what window to show
and when.
