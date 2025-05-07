# self hosted nix cache

self hosted nix cache is cool. what i want is to have my own server, where i can
use this flow:

1. locally want to build a custom package
1. build request gets sent off to remote builders (owned by me)
1. remote builders build and push the build to cache
1. subsequent requests for the package get downloaded from cache

a nix cache is literally just an http server that hosts metadata files and the
actual package archive. when i request a package, nix will check the caches i
have configured and download from one of those if the package is found.

on top of that, i can configure build machines in nix that are maybe larger or
running a different platform (i.e. linux vs mac, x86 vs arm). this way i have a
much wider breadth of builds available to me for effectively free (minus the
cost to build and configure the whole system lol). remote builders can build
multiple packages at the same time so it _should_ overall improve the
performance, especially for large packages.

the remote builders can be configured using the `post-build-hook` to run some
arbitrary script after a successful build. this hook can be used to push
successful builds to the cache server.

with all of this stuff set up, packages will try to hit the cache server first
and if the package isn't found, a build will be sent to a remote builder that
will push the successful build to the cache. this means i get an (ideally)
overall faster and more convenient build experience. it also means i can include
custom tools and custom builds in my standard configurations and not need to
rebuild those packages on every single machine for every new installation and
new update to that package.

## status

current stage: idea
