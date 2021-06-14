%META:TOPICINFO{author=\"kallea\" date=\"1162240426\" format=\"1.0\"
version=\"1.2\"}% %META:TOPICPARENT{name=\"SyncSystem\"}%
[NIPStyle]{.twiki-macro .INCLUDE}

# Sync System 2.0

[]{.twiki-macro .TOC}

## Currently covered

These folders are at this point synchronized using this system: \*
Shared:\* \* Socials:\*

## Concept (what it does)

The second iteration of the Sync System is meant to handle code
submissions from multiple sources, sorted into three major categories:
proposals, localized objects, and temporary objects (discardables).
These three categories are referred to as \"the IgDiPro\", standing for
\"Ignore, Discard, Propose\".

### Proposals

Proposals are simply changes that you\'d like to see in the global
SkotoSphere. You\'ve fixed a typo in a shared shoe, or you\'ve fixed a
bug in a library, or you\'ve added a nice verb. And you want to be a
nice lad/lass and share it with the rest of us. You can propose an
object in two separate ways.

The hard-core way:

    +synctool propose WOENAME

The user interface way:

    +synctool

One thing to remember is that when you add a **new** object to the
SkotoSphere, you **must** propose it the hard-core way. If you do not,
it will simply be chucked aside (renamed to MOVED:\[DATE\]:\[WOENAME\]),
and we both know you don\'t want that. If you are simply making a
change, the system will realize this within 24 hours. Specifically, it
will scan objects once every day, at midnight PST (that\'s 6 am,
server-time). So if you make a change at 8 in the evening, go to sleep,
wake up the next day around 7.30 in the morning, the system will have
observed your change. Your object will now be listed in the synctool web
interface, marked, by default, as \"localized object\" (a.k.a.
\"ignore\"). Simply click \"propose\", and the object will be sent to
the master server for reviewal at the next 6-am-mark.

Except in some cases, when a proposal arrives at H.Q., it is enqueued,
waiting for a reviewer to take a look at the change and decide if the
change makes sense. If it does make sense, the reviewer will push a
button and your version will replace the old one. If it doesn\'t make
sense, the reviewer will push another button, and your version will be
discarded. However, your change will remain always on your game. See
\"temporary objects\" for further information about this.

### Localized objects

Localized objects (objects marked as \"ignore\") are objects which you,
or the system, have decided should stay local to your game. When new
changes arrive from the master, they will be ignored, and your copy of
the object will remain in place. Only in rare instances is this a good
idea. See SyncIgnore for further information on this. One thing to note
is that objects will be **automagically** marked as \"localized
objects\" in two situations: 1 When the system notices that the objects
have been changed on your end. 2 When you have proposed a change, and
your proposal has been declined.

### Temporary objects

Temporary objects (discardables) are objects which, at the next sync
event (6 am), will revert back to using the global version. Temporary
objects are useful when you have been using a localized version of an
object for a while, and decide that you want to use the global version
(due to a bug-fix or similar, or simply because you want to stay as
in-sync with the rest of us as possible; both very good reasons).
Objects will be automagically marked as \"temporary objects\" in one
situation: 1 When you have proposed a change, and your proposal was
**accepted**. At this point, your version and the global version are in
sync.

## Auto-rules

There are two \"rule systems\" cooperating in the new sync system; one
on the slave-side (all games), and one on the master-side (H.Q.). These
rules let you define what to do in specific situations.

### Slave-side rules

Slave-side (the important thing for you, most likely), you can open up
+synctool and there will be a text area where you can type in rules.
Slave-side auto-rules can be things like: \* Always propose any changes
we make on our game in the Socials:Verbs folder. \* Always ignore any
changes we make on our game in the Shared:OurGame folder. (Note that
this example is ridiculous. Any sane game would use a Gamename:Shared
folder instead.)

You can also \"auto-discard\" changes, though the uses for that rule are
questionable. The most obviously useful one is the \"always propose\"
rule. If your game tends to add a lot of verbs and/or adverbs,
auto-proposing Socials:Verbs or Socials, straight off, is a great idea.
You won\'t have to bother with the proposal queues anymore (for that
folder).

### Master-side rules

Master-side (also important, because it explains a few things that you
may notice), it is possible to **auto-approve** changes from a specific
game to a specific folder. This is ultra-nice if you, for example, have
whipped up a cool system and want to share it with the rest of us. Since
it is your system, we would want to auto-approve changes to that
system\'s folder (say, Shared:CoolSystem) from your server. Presuming
you auto-approved that folder as well, when you made changes to the
folder, \* The changes would be auto-proposed to H.Q. \* The proposals
would be auto-accepted by H.Q. \* Thus, your system would be world-wide
available always (or within 24 hours, anyway).

## The inner workings

Warning: this section is very technical, and describes in-depth how the
sync code handles objects, and determines how to treat changes and
similar. You may want to stop reading now unless tech-gore fascinates
you.

Syncing happens once every 24 hours, at midnight PST. The syncing is
divided into two separate cycles, which export and import changes from
the central server H.Q. (headquarters).

The system relies of revisions to determine how to deal with a slave\'s
object if it does not match the master object. This is done by comparing
the latest revision in the slave to the latest revision in the master;
if the revisions match, the objects are considered identical.

### The cycles

Syncing occurs in two cycles; the update-cycle and the pending-cycle, in
that order. \* The update-cycle applies the changes made on H.Q. (the
master) upon all the slaves. This happens in parallel. The update-cycle
is referred to as \"HQ-\>\*\". \* The pending-cycle imports the
proposals and reports from the individual slaves to H.Q. This also
happens in parallel. The pending-cycle is referred to as \"\*-\>HQ\".

### Revision logic

The sync system relies heavily on object revisions to determine whether
an object on a slave has been modified locally or not. Since the risk of
collisions is fairly high (the same object is modified at the same time
on both master and a slave), the sync system defines \"modified
locally\" as follows:

    The last revisions timestamp is newer than the date of the most recent (prior to the current) HQ->* cycle.

There are three possible situations when an existing object is found to
be different from the master object, excluding situations where the
slave object has been marked in certain manners: \* The master object
was modified since the last sync. \* The slave object was modified since
the last sync. \* The master object **and** the slave object were both
modified since the last sync.

When the master object was modified (but not the slave object), the
solution is simple. The slave is replaced with the master. When the
slave object was modified it becomes trickier. The sync system by
default presumes that changes made are changes wanted, but it does not
presume that a change made is a change that should be proposed. In this
case, the sync system will mark the slave object as a localized object
(using the \"ignore\" flag). When both objects were modified, the sync
system will presume that the modifications made to the slave are wanted,
regardless. The master object\'s modifications are in fact not relevant
at this point. This is expressed using the following code segment:

    if ($ob_revision                     /* the timestamp of the slave object */
     && $sync_stamp                   /* the timestamp of the last successful sync */
     && $ob_revision < $sync_stamp  /* slave's latest modification was before the last sync */
     && $revision > $ob_revision)   /* master was modified after the object */

If the if-case falls through, the object is discarded; otherwise, the
object is set to ignore.

However, due to the auto-rules, the object may instead be proposed,
ignored, or discarded by default.

\-- Main.KalleAlm - 25 Oct 2006
