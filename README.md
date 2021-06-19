# Overview

This mod ensures that your leaders can have every species-based leader trait that their species makes them eligible for.  Somehow managed to get a Psionic, Erudite, Cybernetic, Brainslugged species?  Now your leaders can have all of the traits together!

Like the default game, this mod does **not** guarantee psionic traits for Latent Psionics (instead there is a 20% chance for each leader) or brain slugs (again, 20% chance for each leader).

# Changes

This mod unifies adding/removing species-based traits for leaders, and it will automatically fire when leaders are spawned (`on_leader_spawned`), when a species is modified (`on_modification_complete` - will not add latent psionics or brainslugs), or when a ruler returns to their previous leader job (`on_ruler_back_to_pre_ruler_class` - ensures brainslugged/psionic rulers keep their trait(s) when being demoted).  It does not replace the majority of the existing game code for these traits in order to off maximum compatibility with other mods, but it does occasionally result in leaders having their traits in a different order.

The same code is used in each of these three cases (via effects) that do not enforce any requirements other than "does the leader's species have the necessary prerequisite species trait?"  So if you've managed to glitch the game into having Psionic Robots, you'll end up with psionic robot leaders.

The original inspiration for me to create this mod was the "Promising Officer" event which rewards admirals with two traits, but none of their appropriate species traits.  This seems to occur because the built-in effect to spawn a leader `create_leader` completely overwrites any species traits whenever `traits = { }` are specified (as in this event `leader.1`).  Currently, this mod only addresses this one instance, but if you find another place that would benefit from an event-created leader having their species traits added, please leave a comment describing which event.

## Compatibility

This mod overrides one default event related to adding leader traits: `distar.174`.  Other mods that make changes to the same event will conflict with this one.  The easiest way to resolve the conflict is to comment out or delete the event from this mod, which is in the file `events/000_overridden_leader_species_trait_events.txt`.  The override is just a cleaner way to add the brainslug trait - the original in-game code will execute in the two places it could be triggered but they have a very low impact.

In contrast, all other the new logic is implemented in standalone events and effects which should not conflict with other mods.

## Known Issues

Overriding events from the default game causes error logs.  Expect to see one line similar to this in error.log:

```
[22:30:44][eventmanager.cpp:355]: an event with id [distar.174] already exists!  file: events/distant_stars_events.txt line: 5256
```

## Changelog

* 1.0.0 Initial version
* 1.1.0 New admirals gained from the "Promising Officer" event after a victorious battle (`leader.1`) will be given appropriate species traits
* 1.1.1 Updated README, unified event code for randomizing species traits on a new leader
* 1.1.2 Actually update version numbers