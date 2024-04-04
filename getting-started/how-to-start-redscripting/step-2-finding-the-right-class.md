---
description: What game logic will you piggyback on?
---

# Step 2: Finding the right class

## Summary&#x20;

Created: Apr 02 2024 by [manavortex](https://app.gitbook.com/u/NfZBoxGegfUqB33J9HXuCs6PVaC3 "mention")\
Last documented edit: [Angevil](https://app.gitbook.com/u/H2MkhXktQaM12OsjMzjcsLnrDT22 "mention")

This page will list useful classes and tell you where to find more information about them.

## Explanation

When you're completely lost, how do you find the right class?

## Classes (and what they do)

### PlayerPuppet

```swift
public class PlayerPuppet extends ScriptedPuppet
```

This class is the Player and handles most of the events and logic related to player

#### Examples

```swift
protected cb func OnGameAttached() -> Bool
```

This function is called when the GameInstance is attached to the Player

### DamageSystem

```swift
public final native class DamageSystem extends IDamageSystem
```

This class handles all the events and calculations of the damages done to all the gameobjects from all the sources

#### Examples

```swift
private final func ProcessPipeline(hitEvent: ref<gameHitEvent>, cache: ref<CacheData>) -> Void
```

This is the starting event that gets called when an object is hit and it has a `gameHitEvent` variable

