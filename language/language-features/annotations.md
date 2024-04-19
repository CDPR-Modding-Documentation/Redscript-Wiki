---
description: >-
  Annotations are special keywords that you can use to change or extend the
  behavior of existing methods and classes in the base game. You cannot use
  annotations to modify modded methods or classes.
---

# Annotations

{% hint style="warning" %}
You cannot use the following annotations on native functions.
{% endhint %}

{% hint style="info" %}
You can now generate declaration for annotations using [NativeDB](https://nativedb.red4ext.com). You need to configure option _Clipboard syntax_ to Redscript. You can click on the "copy" button of a function, pick a feature and it will copy the code in your clipboard.
{% endhint %}

### @wrapMethod(class)

You can use this annotation to write a new function that supersedes an existing method with the same name and parameters on a specified class. Your function will execute instead of the original method whenever it is invoked, but you can still access the underlying method by using the `wrappedMethod(…)` identifier. You would typically invoke the wrapped method in your newly defined function and forward all the arguments unchanged to preserve it's original behavior, but you can also modify the arguments or the final return value if you wish. Furthermore, this annotation can be applied multiple times to chain multiple functions that modify the same method. Each function in the chain wraps around the previous one. This helps to keep mods compatible even if they alter the same methods. For example, multiple mods can use this annotation to append new buttons to the main menu:

{% code overflow="wrap" lineNumbers="true" %}
```swift
@wrapMethod(SingleplayerMenuGameController)
private func PopulateMenuItemList() {
  wrappedMethod(); // This will call the original PopulateMenuItemList
  this.AddMenuItem("BUTTON 1", n"OnDebug");
}

// You can use this annotation to chain multiple functions
@wrapMethod(SingleplayerMenuGameController)
private func PopulateMenuItemList() {
  wrappedMethod(); // This will run the previous function with this annotation
  this.AddMenuItem("BUTTON 2", n"OnDebug");
}

​// The result will be two new buttons added to the main menu
```
{% endcode %}

{% hint style="info" %}
When you use `wrappedMethod(…)` to call the original method, make sure to match the arguments for the correct overload of the method.

{% code overflow="wrap" lineNumbers="true" %}
```swift
@wrapMethod(SingleplayerMenuGameController)
private func PopulateMenuItemList() {
  let menuItemCount: Int32 = 5;
  // this will pass menuItemCount as an argument to the wrong overload of PopulateMenuItemList
  wrappedMethod(menuItemCount);
}
```
{% endcode %}
{% endhint %}

{% hint style="info" %}
You should usually call `wrappedMethod(…)` in your wrapper function. However, you can use a conditional statement to decide when to call it if you don’t need it in some situations.

{% code overflow="wrap" lineNumbers="true" %}
```swift
@wrapMethod(SingleplayerMenuGameController)
private func PopulateMenuItemList() {
  let menuItemCount: Int32 = 5;
  if menuItemCount > 5 {
    wrappedMethod(menuItemCount); // wrappedMethod(…) will not run because menuItemCount is not greater than 5
  };
}
```
{% endcode %}
{% endhint %}

### @addMethod(class)

You can use this annotation to create a new method and add it to an existing class. For example, you can write a new method to disassemble all junk items in your backpack and add it to the `BackpackMainGameController` class:

<pre class="language-swift" data-overflow="wrap" data-line-numbers><code class="lang-swift">@addMethod(BackpackMainGameController)
private final func DisassembleAllJunkItems() {
<strong>    ...
</strong>}
</code></pre>

### @replaceMethod(class)

You can use this annotation to write a new function that has the same name and parameters as an existing method in the game. Your function will replace the original method and change its behavior.

{% code overflow="wrap" lineNumbers="true" %}
```swift
@replaceMethod(CraftingSystem)
private final func ProcessCraftSkill(xpAmount: Int32, craftedItem: StatsObjectID) {
    ...
}
```
{% endcode %}

{% hint style="info" %}
You can only use one `@replaceMethod` for each method. If you define more than one, only one of them will work. If you want to combine multiple changes to the same method, you should use the `@wrapMethod` annotation instead.
{% endhint %}

{% hint style="info" %}
This annotation is compatible with `@wrapMethod`. It will always overwrite the original method and it will not affect the functions that wrap around it.
{% endhint %}

### @replaceGlobal()

You can use this annotation to write a new function that has the same name and parameters as an existing global function in the game. Your function will replace the original function and change its behavior.

<pre class="language-swift" data-overflow="wrap" data-line-numbers><code class="lang-swift">@replaceGlobal()
public static func CreateExpEvent(amount: Int32, type: gamedataProficiencyType) -> ref&#x3C;ExperiencePointsEvent> {
<strong>    ...
</strong>}
</code></pre>

### @addField(class)

You can use this annotation to create a new field and add it to an existing class. For example, you can add a dummy field of type `Int32` to the `PlayerPuppet` class:

{% code overflow="wrap" %}
```swift
@addField(PlayerPuppet)
let dummy: Int32;
```
{% endcode %}
