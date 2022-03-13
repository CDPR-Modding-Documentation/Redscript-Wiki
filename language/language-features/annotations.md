---
description: >-
  The compiler supports a number of annotations. Their main purpose is to
  add/modify functionality in existing game functions and classes.
---

# Annotations

### @wrapMethod(class)

This annotation needs to be followed by a function definition with a signature matching some existing in-game method. Your function will effectively 'wrap' the existing method on the specified class, i.e. whenever the original method is invoked your function will be executed instead - but it's your function's responsibility to invoke the method being wrapped and you can do it by using the special `wrappedMethod` identifier. You can invoke it by forwarding the original arguments or change them however you wish. On top of that, this annotation allows chaining of these wrappers and incrementally adding functionality to existing methods. This makes it pretty easy to keep mods compatible even when they modify the same methods.

```swift
@wrapMethod(SingleplayerMenuGameController)
private func PopulateMenuItemList() {
  wrappedMethod(); // this will call the original PopulateMenuItemList
  this.AddMenuItem("BUTTON 1", n"OnDebug");
}

// you can chain them - this function will wrap around the previous wrapper
@wrapMethod(SingleplayerMenuGameController)
private func PopulateMenuItemList() {
  wrappedMethod(); // this will call the previous wrapper
  this.AddMenuItem("BUTTON 2", n"OnDebug");
}

// we'll end up with BUTTON 1 and BUTTON 2 added to the main menu
```

### @addMethod(class)

This annotation will add your newly defined method to the specified class.

```swift
@addMethod(BackpackMainGameController)
private final func DisassembleAllJunkItems() {
...
```

### @replaceMethod(class)

This annotation needs to be followed by a function definition with a signature matching some existing in-game method. Your function will effectively replace the existing method.

```swift
@replaceMethod(CraftingSystem)
private final func ProcessCraftSkill(xpAmount: Int32, craftedItem: StatsObjectID) {
...
```

{% hint style="info" %}
If there is more than one @replaceMethod defined for a single method it will lead to only one of them taking effect. If you want to be able to combine multiple modifications you should check the @wrapMethod annotation.
{% endhint %}

{% hint style="info" %}
This annotation **is compatible** with @wrapMethod. It will always replace the original method and it does not affect the wrappers.
{% endhint %}

### @replaceGlobal()

This annotation needs to be followed by a function definition with a signature matching some existing in-game global function. The matching function will be replaced with yours.

```swift
@replaceGlobal()
public static func CreateExpEvent(amount: Int32, type: gamedataProficiencyType) -> ref<ExperiencePointsEvent>
...
```

{% hint style="info" %}
This is known not to work for some native functions.
{% endhint %}

### @addField(class)

This annotation needs to be followed by a field definition that will be added to the target class.

```swift
@addField(PlayerPuppet)
let dummy: Int32;
```
