# The shared clothing system

The shared clothing system intends to become a common core system for
all clothing systems in all games in SkotOS, by allowing customizations
at every point in the UrHierarchy as well as being simple (so as to not
include possibly undesired, extraneous features of any kind).

The shared clothing system theoretically came into effect July 2006.

For the specification proposal, [click
here](Shared clothing specification).

## How to use it in your game

The simplest way to start using a shared item is to spawn it up and use
it. If you want to be fancy, set its traits to appropriate things (e.g.
set \"trait:color\" to \"blue\") either using +setp or using the +traits
popup (+traits my shoes).

If you wish to introduce a shared item into your game, do it like you
would introduce any item into your game. If you have NPC\'s which hand
out/sell items, well, then give the NPC the item(s) you want in the
game, and voila, there you have it. If you need to customize the object,
then simply go into Woe, spawn the item you want, rename it to
Yourgame:clothing:shoes (or whatnot) and modify your local copy, then
put that in game.

If you wish to submit modifications to the item, then please email the
S7 list and we will arrange for your item to be transferred over to the
master server. Eventually you will be able to do this on your own, as a
proposal, but this is currently not possible.

## What it does

The biggest advantage with the shared clothing system is, well, that
it\'s shared. It\'s a big collection of clothing items (which will
continue to grow over time), each item available in all games, and each
item confirmed to use the same, underlying core system. A game basing
its clothing on the shared core clothing system will additionally be
able to use any shared clothing item, as is, in their game. Any new
items added can also be used, as is, with no modifications required
(although modifications are of course always possible to do, at the whim
of the game hosts).

## How that is possible

This is handled via the expansion system inherent in the shared system
itself. The expansion system expands the UrHierarchy, \"weaving in\"
game-local UrParents at every step in the UrHierarchy. Thus, any item
that inherits the Shared:clothing:UrClothingPair is in actuality
inheriting Shared:Local:clothing:UrClothingPair, which in turn inherits
Shared:clothing:UrClothingPair. By doing it this way, the system lets
you put in game-local code or properties into the :UrClothingPair object
itself, by simply doing so in the Shared:Local object.

## What it doesn\'t do

The shared clothing system does not, and will never, include crafting.
While you can customize an item to oblivion via the traits, the shared
core will never include systems for actually letting the players do
this. There are tools which let a game host alter things easily
(+traits) but these tools do not take into account character skills, or
similar. The reason why the shared clothing system will not include this
(and other) features is because it is meant to be exclusively about
clothing. The moment we start adding crafting, or any other external
system to the core, we are taking away the flexibility of the system
itself.

That said, there will over time be many examples to derive your own
crafting code from.

\-- Main.KalleAlm - 27 Jul 2006
