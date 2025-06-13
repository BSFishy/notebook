# storage

storage is something i've kinda been worried about for a while but not something
i've put very much time or effort into worrying about. THAT IS UNTIL NOW

so my goals with storage is primarily redundancy. as with the entire lab. i want
to make sure that regardless of anything, my stuff remains functional. i think i
want data to be replicated across the cluster, backed by a nas, and backed up to
an offsite backup.

to start, i will have a single node in the cluster. at this point, i will not
have any high priority and high value data solely residing in the cluster (i.e.
i will have it backed up somewhere else). however, i want to be able to
incrementally add nodes to the cluster, which should increase both redundancy of
cpu as well as storage. and then in the more far out future, i should be able to
add a nas as the backing store.

to facilitate this, i think i will start out with
[longhorn](https://longhorn.io). this should give me what i want and scale to
pretty large. it should allow me to start with one node, expand to more in the
future, expand to use a nas, and backup to an offsite backup all with native k8s
pvcs.

in the further future, i may expand to rook + ceph for a super duper balls to
the walls insane storage setup that should be faster, more fault tolerant, and
overall better, but that will also be a huge undertaking and something that i
need to be super serious about doing lol.

also, for remote backup, im thinking about using backblaze. might want to chat
with their support to know what would be the proper plan for me, but we'll see.
