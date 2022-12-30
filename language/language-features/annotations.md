---
description: >-
  The compiler supports a number of annotations. Their main purpose is to
  add/modify functionality for methods and classes in the base game only. You
  cannot annotate methods or classes from mods.
---

# Annotations

### @wrapMethod(class)

This annotation needs to be followed by a function definition with a signature matching some existing in-game method. Your function will effectively 'wrap' the existing method on the specified class, i.e. whenever the original method is invoked, your function will be executed instead - but it's your function's responsibility to invoke the method being wrapped, which you can do by using the special `wrappedMethod` identifier. You can invoke it by forwarding the original arguments or change them however you wish. On top of that, this annotation allows chaining of these wrappers and incrementally adding functionality to existing methods. This makes it relatively easy to keep mods compatible even when they modify the same methods.

{% code overflow="wrap" lineNumbers="true" %}
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
{% endcode %}

{% hint style="info" %}
When using the `wrappedMethod(...)` function to call the `PopulateMenuItemList()` function, pass the required arguments (or lack thereof) for the correct overload.

{% code overflow="wrap" lineNumbers="true" %}
```swift
@wrapMethod(SingleplayerMenuGameController)
private func PopulateMenuItemList() {
  let menuItemCount: Int32 = 5;
  // passes menuItemCount as a parameter to the wrong overload - PopulateMenuItemList(count: Int32)
  wrappedMethod(menuItemCount);
}
```
{% endcode %}
{% endhint %}

{% hint style="info" %}
An invocation of `wrappedMethod(...)` should generally be present in a well-defined `@wrapMethod` function. However, you have the option to encapsulate it with a conditional statement where it is only invoked when certain criteria have been met.

{% code overflow="wrap" lineNumbers="true" %}
```swift
@wrapMethod(SingleplayerMenuGameController)
private func PopulateMenuItemList() {
  let menuItemCount: Int32 = 5;
  if menuItemCount > 5 {
    wrappedMethod(menuItemCount); // wrappedMethod(...) is not invoked because the conditional 'if' statement doesn't return true
  };
}
```
{% endcode %}
{% endhint %}

### @addMethod(class)

This annotation will add your newly defined method to the specified class.

<pre class="language-swift" data-overflow="wrap" data-line-numbers><code class="lang-swift">@addMethod(BackpackMainGameController)
private final func DisassembleAllJunkItems() {
<strong>    ...
</strong>}
</code></pre>

### @replaceMethod(class)

This annotation needs to be followed by a function definition with a signature matching some existing in-game method. Your function will effectively replace the existing method.

{% code overflow="wrap" lineNumbers="true" %}
```swift
@replaceMethod(CraftingSystem)
private final func ProcessCraftSkill(xpAmount: Int32, craftedItem: StatsObjectID) {
    ...
}
```
{% endcode %}

{% hint style="info" %}
If there is more than one @replaceMethod defined for a single method it will lead to only one of them taking effect. If you want to be able to combine multiple modifications you should check the @wrapMethod annotation.
{% endhint %}

{% hint style="info" %}
This annotation **is compatible** with @wrapMethod. It will always replace the original method and it does not affect the wrappers.
{% endhint %}

### @replaceGlobal()

This annotation needs to be followed by a function definition with a signature matching some existing in-game global function. The matching function will be replaced with yours.

<pre class="language-swift" data-overflow="wrap" data-line-numbers><code class="lang-swift">@replaceGlobal()
public static func CreateExpEvent(amount: Int32, type: gamedataProficiencyType) -> ref&#x3C;ExperiencePointsEvent> {
<strong>    ...
</strong>}
</code></pre>

{% hint style="info" %}
This is known not to work for some native functions.
{% endhint %}

### @addField(class)

This annotation needs to be followed by a field definition that will be added to the target class.

{% code overflow="wrap" %}
```swift
@addField(PlayerPuppet)
let dummy: Int32;
```
{% endcode %}
