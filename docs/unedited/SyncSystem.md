%META:TOPICINFO{author=\"kallea\" date=\"1162240461\" format=\"1.0\"
version=\"1.4\"}% %META:TOPICPARENT{name=\"WebHome\"}%
[NIPStyle]{.twiki-macro .INCLUDE}

# The 2.0 Sync System

See also the more informative and less introductory topic on this
system, SyncSystemTwo

*In the beginning, the Earth was without form, and void\....* and there
was one master and many slaves. The master, as most of us know, was the
game **Castle Marrach** and the slaves were, well, everybody else.

This meant a few things: 1 The master team (the Marrach staff) were
burdened with handling all global stuff\... like socials, all \"global
code\", and so on. Naturally, people like Aziel and Tony and myself have
helped out with that part since we have access to Marrach. Which leads
to, 2 Only the master team **can** handle global stuff. If the
bright-as-a-clam Lucy wants to fix a bug or add a verb or change a word
in a sentence in a shared piece of clothing \... she can\'t. Unless
she\'s a staffer in Marrach. 3 The master team dictatorially chose the
content in the SkotoSphere. If 5 games wanted to have a fart verb, and
the master team didn\'t, well, the 5 games would simply have to make 1
fart verb object in each of those 5 games, instead of using a global
one. More seriously, however, is the case where a slave game does
**not** want an object or feature that the master is syncing. Even if
all other slaves want this one object, this slave is now forced to use
it. With all that this entails. 4 Experimenting was discouraged on
slave-games and encouraged on the master \-- the master in this case
being a live game. Experimenting on a live game should be
**discouraged** not the other way around.

*But the Sun shone upon the sleeping Earth and deep inside the brittle
crust massive forces waited to be unleashed.* The new sync system
handles things quite a bit differently:

\* The master is not a live game. In fact, the master is a new shack
called \"Head quarters\" (HQ for short), which is dedicated to becoming
the next content master, among other things. \* Modifications (add a
\"nose\" to the shared ur-human object) and additions (add the verb
\"moogle\") are proposed **from slaves** and judged and applied to the
main system **in head quarters**. This means that any game, anywhere can
submit stuff to be included in the SkotoSphere. \* \"Game-local versions
of objects\" are possible. \* Syncs happen once every day instead of
once every week.

*The seas parted and great continents were formed. The continents
shifted, mountains arose. Earthquakes spawned massive tidal waves.
Volcanoes erupted and spewed forth fiery lava and charged the atmosphere
with strange gases.* How does this all work, then? Well, this is the
typical order of events from when you change an object to when your
changes become used by the whole world: 1 You modify an object that is
in the shared SkotoSphere (this currently means the \"Shared\" folder
only, but this will grow with time to include all the others). 2 The
next sync happens. Uh-oh! Your change will be destroyed, right? Wrong.
The sync will compare the revisions of the master object and your
version, and will see, \"Oh hey hey hey, this is newer than the master
version\" So the object is **marked** by the sync system and added to a
queue. The mark means that the object will be **ignored** at the next
sync, and will continue to be ignored; any changes made on the master
(HQ) will not affect the slave (your modified version). 3 Eventually,
you haul your lazy ass into the +synctool system and spot your modified
object sitting there. Since you **do want to share your changes** you
change its mark to **propose**. If you **didn\'t** want to share your
changes, i.e. you wished to keep your object, then you would have left
it as **ignore**. This is a perfectly valid thing to do, but involves
some potential issues. See SyncIgnore for further details on why. 4 When
you click that **propose** link, a number of things happen under the
hood that you can completely ignore. The sum of it all though is that
your modified object is enqueued and, within 24 hours, it is chucked off
to head quarters. 5 A spunky HQ reviewer named Bert finds your proposal
and thinks to himself, \"Oh goody, this will save the world\" so he
approves. Gears turn under the hood and at the next sync (within 24
hours), the new object is sent out to **all slaves in Skotos**. 6 The
object arrives in your game, with your modification added to it. The
sync system observes that your change was indeed applied, so it
**discards** your local copy in favor of the master copy. (Because the
master copy contains your improvements.)

*Into this swirling maelstrom of Fire and Air and Water the first
stirrings of Life appeared: tiny organisms, cells, and amoeba, clinging
to tiny sheltered habitats.* Now, you of course wonder, \"But what if
that spunky lad Bert hadn\'t thought so highly of my proposal?\" This is
what would have happened then: 5 A spunky HQ reviewer named Bert finds
your proposal and thinks to himself, \"Uh\... huh?\" so he declines.
Gears turn under the hood and at the next sync the **same** object is
sent out to your game along with a little message that says \"Your
proposal for the XYZ object was declined.\" 6 The object arrives and the
sync system observes that your change was **not** applied, so it
**ignores** the master copy in favor of **your local copy**. The sync
system will reason that if you feel enough about your change that you
want to propose it, you probably want to keep it at least for yourself,
even if nobody else wants to see it.

*But the seeds of Life grew, and strengthened, and spread, and
diversified, and prospered, and soon every continent and climate teemed
with Life.* There\'s another, third mark which you can use called
**discard**. This simply means that you want to revert to using the
master copy of a specific object, rather than using your own copy. It\'s
almost always a bad idea to use a local copy of an object unless you
really have to. Any changes (such as bug fixes or improvements) made to
an object that you have in the **ignored** state will be lost to you
until you discard that object.

**Please observe that this system only applies to the Shared: folder and
the Socials: folder at this point in time. It is experimental and as
soon as we have proven that it works and is bugfree, we will gradually
include other folders, such as Lib, Neoct, etc.**

SyncImprovements is a topic dedicated to \"what is to come, possibly, if
the sun shines brightly upon us and the word is green.\"

\-- Main.KalleAlm - 06 Oct 2006

*A free beer to whoever can name the origin of the quotes used above \--
and a big lamer award to whoever uses google (et al) to find out :)*
