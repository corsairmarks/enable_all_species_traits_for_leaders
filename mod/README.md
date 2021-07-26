# Overview

This mod ensures that your leaders can have every species-based leader trait that their species makes them eligible for.  Somehow managed to get a Psionic, Erudite, Cybernetic, Brainslugged species?  Now your leaders can have all of the traits together!

Like the default game, this mod does **not** guarantee psionic traits for Latent Psionics (instead there is a 20% chance for each leader) or brain slugs (again, 20% chance for each leader).  The game includes an event for leaders of a Latent Psionic species randomly gaining the Psionic leader trait, but it was limited to the main species of an empire.  That has been altered so any Latent Psionic leaders have a chance to randomly manifest Psionic abilities.  Additionally, an event functionally similar has been added for leaders from a species with Brain Slugs to gain the Brain Slug trait.

# Changes

This mod unifies adding/removing species-based traits for leaders, and it will automatically fire when leaders are spawned (`on_leader_spawned`), when a species is modified (`on_modification_complete` - will not add latent psionics or brainslugs), or when a ruler returns to their previous leader job (`on_ruler_back_to_pre_ruler_class` - ensures brainslugged/psionic rulers keep their trait(s) when being demoted).  It does not replace the majority of the existing game code for these traits in order to off maximum compatibility with other mods, but it does occasionally result in leaders having their traits in a different order.

The same code is used in each of these three cases (via effects) that do not enforce any requirements other than "does the leader's species have the necessary prerequisite species trait?"  So if you've managed to glitch the game into having Psionic Robots, you'll end up with psionic robot leaders.

The original inspiration for me to create this mod was the "Promising Officer" event which rewards admirals with two traits, but none of their appropriate species traits.  This seems to occur because the built-in effect to spawn a leader `create_leader` completely overwrites any species traits whenever `traits = { }` are specified (as in this event `leader.1`).  Currently, this mod only addresses this one instance, but if you find another place that would benefit from an event-created leader having their species traits added, please leave a comment describing which event.

## Compatibility

This mod overrides two default events related to adding leader traits: `distar.174` and `utopia.2605`.  Other mods that make changes to one or both of the same events will conflict with this one.

For `distar.174`, the easiest way to resolve the conflict is to comment out or delete the event from this mod, which is in the file `events/000_overridden_leader_species_trait_events.txt`.  The override is just a cleaner way to add the initial brainslug trait for hireable leaders - the original in-game code will execute in the two places it could be triggered but they have a very low impact.

`utopia.2605` is the event for leaders randomly gaining the Psionic trait from Psionic species. I recommend you use this mod's version of the event in order to benefit from the reduced mean time-to-happen (i.e. happens more frequently) and wider selection criteria (any species with Latent Psionic, not just the owner's main species).

All other the new logic is implemented in standalone events and effects which should not conflict with other mods.

### Recommended Companion Mods

Like leaders?  Try a couple of my other leader-related mods that work with this one.

* [Leader Traits: Scientist AI Assistant Upgrader](https://steamcommunity.com/sharedfiles/filedetails/?id=2498166286) will help your scientists upgrade to "Sapient AI Assistants" from "Custom AI Assistants" when you discover the right technology
* [Leader Traits: Enhanced Randomisation](https://steamcommunity.com/sharedfiles/filedetails/?id=2553806265) will help your leaders get species-appropriate traits (no more substance abusing robots!) and can randomly get _any_ of the class-appropriate traits when levelling up
* [Retain Leaders from Integrated Subjects](https://steamcommunity.com/sharedfiles/filedetails/?id=2553818684) will let you choose whether you would like to keep leaders from integrated subjects or conquered/infiltrated primitives

### When to Install

This mod can be safely added or removed from your save game after the game has started.  It is implemented through custom events and effects without adding gameplay objects.  If you remove it, your game will work normally.

## Known Issues

Overriding events from the default game causes error logs.  Expect to see two lines similar to this in error.log:

```
[13:36:36][eventmanager.cpp:355]: an event with id [distar.174] already exists!  file: events/distant_stars_events.txt line: 5256
[13:36:37][eventmanager.cpp:355]: an event with id [utopia.2605] already exists!  file: events/utopia_on_action_events.txt line: 1456
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

## Source Code

Hosted on [GitHub](https://github.com/corsairmarks/enable_all_species_traits_for_leaders)

### Development Notes

It is best to clone this repository into `<Stellaris User's Directory>/Paradox Interactive/Stellaris/mod`, and then make a connection to the `mod` folder via a `*.mod` file's `path` property.  That will ensure the game can see the files, and also that CWTools will parse them.  Also note that the README.md file exists in the `mod` directory but is symbolically linked in the root directory.
