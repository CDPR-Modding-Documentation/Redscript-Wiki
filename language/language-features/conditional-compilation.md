# Conditional compilation

Conditional compilation lets you write code that adapts to statically known information like presence or absence of other mods. For example, you might want to use a feature from another mod, but only if the user has that mod installed. Or you might want to avoid a conflict with another mod by changing your code depending on whether that mod is present or not.

To use conditional compilation, you need to use the `@if` annotation followed by a condition before the code that you want to include or skip. Here’s a simple example:

```swift
module My.Mod
// This line will only import Other.Mod if it exists
@if(ModuleExists("Other.Mod"))
import Other.Mod.*

// This function has two versions: one for when Other.Mod exists and one for when it doesn't
// The imports from Other.Mod are only valid in the first version, because they depend on the condition
@if(ModuleExists("Other.Mod"))
func Testing() -> Int32 {
    return 1;
}
@if(!ModuleExists("Other.Mod"))
func Testing() -> Int32 {
    return 2;
}

// This method wrapper/replacement only take effect if Other.Mod doesn't exist
@if(!ModuleExists("Other.Mod"))
@wrapMethod(PlayerPuppet)
protected cb func OnGameAttached() -> Bool {
  Log("I'm in a wrapper");
  wrappedMethod();
}
```

The condition in the `@if` annotation can only consist of:

* `true` or `false`
* logical operators (&&, ||, !, ?:)
* the `ModuleExists` function, which returns true if a mod name exists and false otherwise
