# Overview

This mod ensures that your leaders can have every species-based leader trait that their species makes them eligible for.  Somehow managed to get a Psionic, Erudite, Cybernetic, Brainslugged species?  Now your leaders can have all of the traits together!

Like the default game, this mod does **not** guarantee psionic traits for Latent Psionics (instead there is a 20% chance for each leader) or brain slugs (again, 20% chance for each leader).  The game includes an event for leaders of a Latent Psionic species randomly gaining the Psionic leader trait, but it was limited to the main species of an empire.  That has been altered so any Latent Psionic leaders have a chance to randomly manifest Psionic abilities.  Additionally, an event functionally similar has been added for leaders from a species with Brain Slugs to gain the Brain Slug trait.

Governors whose species has the "Numistic Administration" trait gain the "Merchant of Numa" leader trait.

# Changes

This mod unifies adding/removing species-based traits for leaders, and it will automatically fire when leaders are spawned (`on_leader_spawned`) or when a species is modified (`on_modification_complete` - including a one-time, independent chance for latent psionics or brainslugs).  It does not replace the majority of the existing game code for these traits in order to off maximum compatibility with other mods, but it does occasionally result in leaders having their traits in a different order.

The same code is used in each of these three cases (via effects) that do not enforce any requirements other than "does the leader's species have the necessary prerequisite species trait?"  So if you've managed to glitch the game into having Psionic Robots, you'll end up with psionic robot leaders.

The original inspiration for me to create this mod was the "Promising Officer" event which rewards admirals with two traits, but none of their appropriate species traits.  This seems to occur because the built-in effect to spawn a leader `create_leader` completely overwrites any species traits whenever `traits = { }` are specified (as in this event `leader.1`).  Currently, this mod only addresses this one instance, but if you find another place that would benefit from an event-created leader having their species traits added, please leave a comment describing which event.

## Compatibility

Built for Stellaris version 3.8 "Gemini."  Not compatible with achievements.

This mod overrides eight default events related to adding leader traits: `distar.170`, `distar.171`, `distar.172`, `distar.173`, `distar.174`, `utopia.2508`, `utopia.2601`, and `utopia.2605`.  Other mods that make changes to any of the same events will conflict with this one.  For this mod, these events are stored in the file `events/000_leader_species_traits_event_overrides.txt`.

1. For `distar.170`, `distar.171`, and `distar.172` you should keep the versions from this mod or otherwise merge the changes - they enable cybernetic empires to complete the special project for brainslugs, and ensure robotic leaders aren't selected to receive the brainslug trait.
2. For `distar.173` and/or `distar.174`, the easiest way to resolve the conflict is to comment out or delete the event(s) from this mod.  The overrides are just a cleaner way to add the initial brainslug trait for hireable leaders. If removed or disabled, the original in-game code will execute instead but it has a very low impact.
3. `utopia.2508` is the default event for adding or removing Erudite leader traits.  That event uses the effect `remove_leader_traits_after_modification` which is also overridden.  If you have conflicts with these, it is recommended you keep at least one of the two overrides from this mod, otherwise you will lose Erudite-based leader traits from leaders that also have another special species trait, any time that _any_ species in your empire is modified.
4. `utopia.2601` ensures that newly-spawned latent psionic leaders are flagged as having already rolled for a chance at a psionic trait. If removed or disabled, the flag won't be set so some latent psionic leaders may be able to get a second chance at randomly rolling a psionic trait.
5. `utopia.2605` is the event for leaders randomly gaining the Psionic trait from Psionic species.  I recommend you use this mod's version of the event in order to benefit from the reduced mean time-to-happen (i.e. happens more frequently) and wider selection criteria (any species with Latent Psionic, not just the owner's main species).

All other the new logic is implemented in standalone events and effects which should not conflict with other mods.

### When to Install

This mod can be safely added or removed from your savegame after the game has started.  It is implemented through custom events and effects without adding gameplay objects.  If you remove it, your game will work normally.

### Recommended Companion Mods

Like leaders?  Try some of my other leader-related mods that work with this one.

* [Leader Traits: Merge-Add Species Traits](https://steamcommunity.com/workshop/filedetails/?id=2784224276) re-enables combining species traits like Cybernetic and Psionic via applying species templates, so that it is easier to create a superspecies with multiple leader-beneficial species traits
* [Leader Traits: Scientist AI Assistant Upgrader](https://steamcommunity.com/workshop/filedetails/?id=2498166286) helps your scientists upgrade to "Sapient AI Assistants" from "Custom AI Assistants" when you discover the right technology
* [Retain Leaders from Integrated Subjects & Pre-FTL Civilizations](https://steamcommunity.com/workshop/filedetails/?id=2553818684) lets you choose whether you would like to keep leaders from integrated subjects or conquered/infiltrated primitives
* [[JP localize patch]Leader Traits: All Eligible Species Traits](https://steamcommunity.com/workshop/filedetails/?id=2569179425) Japanese localisation by Dryus

## Known Issues

Overriding events and effects from the default game causes error logs.  Expect to see nine lines similar to this in error.log:

```
[00:39:58][game_singleobjectdatabase.h:153]: Object with key: remove_leader_traits_after_modification already exists, using the one at  file: common/scripted_effects/99_leader_species_traits_scripted_effect_overrides.txt line: 2
[00:40:00][eventmanager.cpp:369]: an event with id [distar.170] already exists!  file: events/distant_stars_events_1.txt line: 5061
[00:40:00][eventmanager.cpp:369]: an event with id [distar.171] already exists!  file: events/distant_stars_events_1.txt line: 5148
[00:40:00][eventmanager.cpp:369]: an event with id [distar.172] already exists!  file: events/distant_stars_events_1.txt line: 5192
[00:40:00][eventmanager.cpp:369]: an event with id [distar.173] already exists!  file: events/distant_stars_events_1.txt line: 5356
[00:40:00][eventmanager.cpp:369]: an event with id [distar.174] already exists!  file: events/distant_stars_events_1.txt line: 5401
[00:40:01][eventmanager.cpp:369]: an event with id [utopia.2508] already exists!  file: events/utopia_on_action_events.txt line: 634
[00:40:01][eventmanager.cpp:369]: an event with id [utopia.2601] already exists!  file: events/utopia_on_action_events.txt line: 1290
[00:40:01][eventmanager.cpp:369]: an event with id [utopia.2605] already exists!  file: events/utopia_on_action_events.txt line: 1378
```

## Changelog

* 1.0.0 Initial version
* 1.1.0 New admirals gained from the "Promising Officer" event after a victorious battle (`leader.1`) will be given appropriate species traits
* 1.1.1 Updated README, unified event code for randomizing species traits on a new leader
* 1.1.2 Actually update version numbers
* 1.2.0 Add event to flag mod as installed
* 1.2.1 Remove extra images files to keep distribution lightweight (no script changes)
* 1.3.0 Remove monthly pulse for mod flag
    * Enhance compatibility for other mods to indicate when a leader's species has changed
    * Renamed "Leader Traits: All Species Traits Eligible" and new thumbnail
    * More preview images
* 1.4.0 Latent Psionics and Brainslugs
    * Improve the event to gain the Psionic trait for Latent Psionic species `utopia.2605` to allow _any_ Latent Psionic species' leaders to gain the trait, not only the main species of an empire
    * Add event based on `utopia.2605` for leaders to randomly gain the Brain Slug trait
    * Reduced mean time-to-happen of both of the above events to 10 years (down from 18 years, 4 months)
* 1.4.1 Minor bugfixes
    * Existing leaders now get a once-per-trait, independent chance for gaining (latent) Psionic or Brainslugged (rather than excluding these traits)
    * Properly restrict trait changes to the _exact_ species that was modified
* 1.5.0 Mark as compatible for Stellaris 3.1 "Lem" - no script changes
* 2.0.0 Add handling for Clone Army admiral traits
* 2.0.1 Fix issue where the Erudite leader trait was deleted from species that had a combo of Erudite plus one or more of Latent Psionic, Psionic, or Cybernetic when other species were modified
* 2.1.0 Be more automagical
    * Add events to adjust leader species-based traits when the game starts and also if the mod is added during gameplay
    * Allow Machine Units to keep Synthetic leader traits if Leader Traits: Synthetic Leader Traits for Machine Units is also installed
* 2.2.0 Mark as compatible for Stellaris 3.2 "Herbert" - no script changes
* 2.2.1 Fix Clone Soldier bug that didn't take into account leader class
* 2.3.0 Mark as compatible for Stellaris 3.3 "Libra" - no script changes
* 2.4.0 Governors from species with the "Numistic Administration" trait gain the "Merchant of Numa" leader trait
* 3.0.0 Update for Stellaris version 3.4 "Cepheus" - use memory optimization feature for effects
* 4.0.0 Update for Stellaris version 3.6 "Orion" (and changes from version 3.5 "Fornax")
    * Apply Synthetic leader traits to machine leaders that qualify (added to the base game in 3.6) - previously, this was part of a companion mod
    * Properly allow _all_ non-robotic leaders to roll for brainslugs in countries that have completed the special project
    * Ensure leaders have the corresponding patron-specific ruler and class-specific "Chosen" traits when possible
    * Ensure leaders with any "Chosen" traits do not have the regular, non-"Chosen" psionic traits
* 4.0.1 Fix a bug in the logic when determining whether to add Synthetic leader traits
* 5.0.0 Add a compatibility trigger for other mods to check whether this one is active, remove old compatibility global flag
* 5.1.0 Add overrides of more brainslug events so that cybernetic empires (including Driven Assimilators) can complete the project
* 6.0.0 Update for Stellaris version 3.7 "Canis Minor" - integrate underlying game changes
* 7.0.0 Update for Stellaris version 3.8 "Gemini"
    * Integrate underlying game changes (lots of code simplification)
    * Do not attempt to adjust leader traits for Clone Soldiers if [Leader Traits: Clone Army - All Leader Classes](https://steamcommunity.com/sharedfiles/filedetails/?id=2784215835) is active

## Source Code

Hosted on [GitHub](https://github.com/corsairmarks/enable_all_species_traits_for_leaders)

### Development Notes

It is best to clone this repository into `<Stellaris User's Directory>/Paradox Interactive/Stellaris/mod`, and then make a connection to the `mod` folder via a `*.mod` file's `path` property.  That will ensure the game can see the files, and also that CWTools will parse them.  Also note that the README.md file exists in the `mod` directory but is symbolically linked in the root directory.