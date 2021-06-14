%META:TOPICINFO{author=\"kallea\" date=\"1160158500\" format=\"1.0\"
version=\"1.1\"}% %META:TOPICPARENT{name=\"SyncSystem\"}%
[NIPStyle]{.twiki-macro .INCLUDE}

# Sync Ignore mode

With the new 2.0 sync system, it is possible to flag objects that are
normally **globally distributed** (that is, they are updated and
controlled by the SkotoSphere) as **ignored**.

Ignored, in this case, means that the objects are left out of the sync
cycle. They are effectively no longer synced from head quarters. Any
changes made to the object are ignored, which additionally means you
have the ability to change the object to your liking.

Suppose that the global verb \"fart\" is a verb you could rather live
without in your game. Understandably. Since the verb is globally
distributed, it is impossible for you to actually delete the verb,
because within 24 hours, the verb will pop up again, happy as a clam,
and ready to be used and abused in your game.

So what can you do? You can set the \"disabled\" flag in the verb. This
will disable the verb entirely, and you will be able to live your life
in your fart-free world happily.

There are a number of problems with doing this, though. The most serious
one can be explained with the following example:

You modify the Lib:NIP:lib:eating library in the NIP distribution,
changing how it works to better suit your game. Your modification is
well written, but you decide that it is only really useful for **your
game**. You set the object\'s mark to **ignore** and then leave it at
that.

Weeks go by, and a bug in the eating library is discovered, and fixed,
in the global distribution. The maintainer of NIP is a sloppy bastard
who forgets to announce this bug-fix to anyone, so time goes on, the bug
remains in **your** version, but not in anybody else\'s. Unfortunately,
the bug also included some changes in how the code interacted with other
code. You realize this, and make slight modifications to a few other
libraries in which you spot bugs. Again, you do not **propose** because
your changes only really benefit your own game.

Evil circle commences. When you make local versions of global code, you
create dependencies that eventually break, forcing you to patch other
code, causing new dependencies which in turn break other dependencies,
ad infinitum. The good solution? In this case, NIP is suited for
\"game-local\" copies of code already, so all you\'d really have to do
is move Lib:NIP:lib:eating to \[yourgame\]:Lib:NIP:lib:eating and make
the changes there. Even better would be to create a new property
container, set its UrParent to Lib:NIP:lib:eating and then put in the
modified properties/scripts into that propcontainer only.

When you don\'t have that option, the best bet is usually to talk to the
maintainer of the piece of code you want changed. If your change is only
beneficial to your game, it might, with some additional effort, become
more dynamic and useful to others, or it may become a customization to
the globally distributed system, which is a much better idea.

\-- Main.KalleAlm - 06 Oct 2006
