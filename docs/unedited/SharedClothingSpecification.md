%META:TOPICINFO{author=\"ChristopherAllen\" date=\"1597911429\"
format=\"1.0\" version=\"1.2\"}%
%META:TOPICPARENT{name=\"SharedClothing\"}%

    From:    Kalle Alm <kalle@mortalis.skotos.net>
    Reply-To:    List for Skotos Seven developers and their cohorts <skotos-seven@skotos.net>
    To:  List for Skotos Seven developers and their cohorts <skotos-seven@skotos.net>
    Subject:     [skotos-seven] Shared clothing rev 2.
    Date:    Tue, 18 Jul 2006 19:22:44 +0200


    Picking up the ball, here's the second revision of the shared clothing
    proposal. I've left things in that we agreed about, and modified the
    things we didn't, and added the things that we came up with.

    The goal for it is to allow new games to benefit from a shared common
    set of clothing as a starting point. While it is not intended as a
    sophisticated crafting system by itself, it is written to faciliate the
    injection and overriding via game-local UrParent practices code and
    properties which makes it compatible with potentially any crafting
    system in the skotosphere.

    Lets start with a simple Ur-style dress, that was copied to
    Shared:clothing:contemporary:dresses:sport

    In KarMode it has three details as follows:

    ======================================================================
    DETAIL [default]
    Brief: white cotton sport dress
    Look: [*
      A clean, short-sleeved sport dress made of white cotton.
    *]
    Examine: [*
      This clean white cotton dress is tailored to fit comfortably about a
    woman's figure; it has short sleeves, a modest neckline and a pleated
    knee-length skirt.
    *]
    Pbrief: clothings
    SName: dress
    SName: garb
    SName: clothing
    SName: garment
    PName: clothes
    PName: clothings
    PName: dresses
    PName: garments
    Adjective: cotton
    Adjective: knee-length
    Adjective: short-sleeved
    Adjective: sport
    ----------------------------------------------------------------------
    DETAIL [skirt]
    Brief: the skirt of a sport dress
    Look: The pleated skirt of a sport dress.
    Examine: [*
      The skirt of a sport dress is pleated and reaches knee-length.
    *]
    SName: skirt
    PName: skirts
    Adjective: knee-length
    Adjective: pleated
    ----------------------------------------------------------------------
    DETAIL [sleeves]
    Brief: the sleeves of a sport dress
    Look: The short sleeves of a sport dress.
    Examine: [*
      The sleeves of a sport dress are short, cut to reach a third of the
    way
    down a wearer's upper arm.
    *]
    SName: sleeve
    PName: sleeves
    Adjective: short
    ======================================================================

    To convert it to the Shared clothing system, we add the following
    properties:

    <Core:Property
    property="trait:material">"cotton"</Core:Property>
    <Core:Property property="trait:color">"white"</Core:Property>
    <Core:Property
    property="trait:condition">"clean"</Core:Property>
    <Core:Property property="export:traits:adj-map">
              ([ "color" : ({ "default" }), "material" : ({ "default" }),
    "condition" : ({ "default" }) ])
    </Core:Property>

    The first are the values for simple traits that all Shared clothing
    should have, and the export:traits is a mapping that allows game
    specific crafting systems to know which traits can be possibly changed,
    and the shared:proofed shows that this item has not been proofed yet.

    Different from revision 1 is the fact that the traits are no longer
    exported. The system will turn traits into initial properties when set.
    Additionally, the export:traits:adj-map property was added (replacing
    adj-list), pointing the specific traits to the specific details for
    adjective usage. Setting this property up is *optional* unless the
    builder wishes to apply adjectives to other than the default detail.
    Furthermore, I want to see a way to do details:[detail]:description:part
    pointing to a particular trait part, and that somehow updating the
    adj-map. Finally the shared:proofed property was removed. Any code
    depending on proofing will check for nil rather than 0, and this will
    save everybody some time in the long run. 

    So now the descriptions for the dress are changed, mostly replacing the
    condition, color, and material with property references, and any text
    tweaking to make it work. You also should delete any adjectives refering
    to the condition, color, and material.

    ======================================================================
    DETAIL [default]
    Brief: $(This.trait:color) $(This.trait:material) sport dress
    Look: [*
      A $(This.trait:condition), short-sleeved sport dress made of
    $(This.trait:color) $(This.trait:material).
    *]
    Examine: [*
      This $(This.trait:condition) $(This.trait:color)
    $(This.trait:material) dress is tailored to fit comfortably about a
    woman's figure; it has short sleeves, a modest neckline and a pleated
    knee-length skirt.
    *]
    Pbrief: clothings
    SName: dress
    SName: garb
    SName: clothing
    SName: garment
    PName: clothes
    PName: clothings
    PName: dresses
    PName: garments
    Adjective: knee-length
    Adjective: short-sleeved
    Adjective: sport
    ----------------------------------------------------------------------
    DETAIL [skirt]
    Brief: the skirt of a sport dress
    Look: The pleated skirt of a sport dress.
    Examine: [*
      The skirt of a sport dress is pleated and reaches knee-length.
    *]
    SName: skirt
    PName: skirts
    Adjective: knee-length
    Adjective: pleated
    ----------------------------------------------------------------------
    DETAIL [sleeves]
    Brief: the sleeves of a sport dress
    Look: The short sleeves of a sport dress.
    Examine: [*
      The sleeves of a sport dress are short, cut to reach a third of the
    way down a wearer's upper arm.
    *]
    SName: sleeve
    PName: sleeves
    Adjective: short
    ======================================================================

    Finally, the dress's UrParent is changed to Shared:clothing:UrClothing,
    which has the following scripts:

    Core:Property   [property='merry:setprop-post:trait']
              return shared_clothing::process_trait($object: this, $property:
    $(hook-property), $old: $(hook-oldvalue), $new: $(hook-value));

    Core:Property   [property='merry:act:start']
              return shared_clothing::construct($object: this);

    The process_trait() function would essentially do four things. 
    1. It would init-prop the property for use by potential children.
    2. It would store the old property as old-[property] in the object
    ("trait:color" -> "old-trait:color").
    3. It would apply the various adjectives based on the traits:adj-map
    property, if [ this."core:ur:firstchild" == nil ]. 
    4. It would trigger a (delayed) function that stepped through all its
    children and, if nil, set the property to the default value.

    The construct() function would basically just ensure that the adjectives
    are not present in the parent. (I have tested and verified that initial
    properties in conjunction with setprop-post scripts function correctly.)

    So how does someone change these traits? Well any staffer can just
    +setprop, or any script change the properties, and the process_trait
    script in setprop-post will just do the right thing. We can also create
    a +sharedmorph command (maybe @morph for chattheatres) that presents
    ajax popup that lets you change all the values listed in traits:list,
    dynamically showing you the result.

    So how do we change Bilbo-based Generic clothing with these?

    First, we run a command on it, +generictoshared (to be written), which
    copies the Base:InitialProperties to recreate normal descriptions with
    place holders for various generic traits (converting trait:color by
    init-propping it, etc.) and converting unsupported traits
    with the text in all caps, like TRAIT:FIT. Finally, the script clears
    out Bilbo code from the initial and core properties. Then you just go
    through the same procedure above to clean up the clothing to the
    simple Shared:clothing:* system.

    A few notes:

    Revision 1 notes (modified & updated):

    * The intent here is to allow StoryBuilders and StoryPlotters without
    merry experience to easily convert or make these clothes. So other than
    adding the 3 properties in XML mode (trait:color, trait:material,
    trait:condition), and changing the UrParent in the E) mode, all of the
    changes can be done in KarMode. 

    * All clothing with initial values should have non-plain values, such as
    white (color) cotton (material). However, this is optional and up to the
    builder.

    * All briefs should:
              * mandatory display color, if any (no matter how small it is, in
    a brief moment you can see its color).
              * optionally display material, if any (often you can't identify
    the material, however).
              * optionally display condition, if any.

    * All looks should have all three values somehow.

    * Children of these clothes can be made by overriding initialized
    property values, but should have a longer name, e.g.:
              UrParent: Shared:clothing:contemporary:dresses:sport
              Children:
                         Shared:clothing:contemporary:dresses:sport-denim-blue-clean
                         Shared:clothing:contemporary:dresses:sport-cotton-pink-clean
                                      
    * At some point, Shared:clothing:* that have been proofed (i.e.
    export:shared:proofed set to 1) might be available from a UI list (say
    for a chat theatre or stage), so the briefs and looks should always be
    valid for all clothes, parent and child, in the Shared:clothing folders.

    * Proofers should check and remove all InitialProperties and any errant
    properties.

    * Proofers should double-check that we really need all the details, or
    maybe merge some details.

    * Could be we want to create some type of popup to help proofers clear
    errant properties, inspect values, etc., and finally approve the item.


    Revision 2 notes:

    * The clothing system will perform a delayed, automatic "expansion" of
    UrInheritance, which basically does the following:
              1. Recursively scan through all clothing objects (that have a woename
    -- i.e. it won't go into spawns). 
              2. Ensure that all objects relay their named children to a game-local
    Data property in Data:Shared.
              3. Create corresponding initial objects for the synchronization of the
    above.

    Effectively, the above will allow games to adjust properties, set
    values, add/override scripts, etc. on any level in the UrChain and still
    use the existing shared objects + their corresponding UrChildren
    (existing as well as those made in the future). For example, this is a
    potential UrChain (child -> parent):
              Shared:clothing:modern:mittens ->
              Data:Shared:clothing:UrPair ->
              Shared:clothing:modern:UrPair ->
              Data:Shared:clothing:UrClothing ->
              Shared:clothing:UrClothing.
    Noone will have to care about setting the urparents appropriately, due
    to the "expansion" mentioned above.

    * The traits:adj-map will be dynamically created and default to the
    default detail of the object (its prime) if an entry does not exist for
    a particular trait. Thus, if adj-map["color"] is nil, and trait:color is
    set, adj-map["color"] is set to ({ "default" }), and the default detail
    acquires the adjective for the color. At this point, the
    details:[detail]:description:part description remains on the wishlist,
    but some form of "check thru all the details before applying the
    default" deal might be the way to go.

    I think that about covers it. Miss anything?

    -Kalle.

\-- Main.KalleAlm - 27 Jul 2006

\[MOVED to Github\]
