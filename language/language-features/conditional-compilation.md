# Conditional compilation

REDscript supports conditional compilation, which can be used to handle complex cases of conflicting functionality between different modules. It allows you to compile different versions of your code based on some information that is available to the compiler. It's achieved through the use of a special @if annotation on top-level declarations and imports.

```swift
module My.Mod
// conditional import
// it'll be available only when the module is present
@if(ModuleExists("Other.Mod"))
import Other.Mod.*

// you can write two different versions of a function based on existence of another module
// imports from Other.Mod will be available only in the first version, since the import was conditional
@if(ModuleExists("Other.Mod"))
func Testing() -> Int32 {
    return 1;
}
@if(!ModuleExists("Other.Mod"))
func Testing() -> Int32 {
    return 2;
}

// you can also conditionally define a method wrapper/replacement
@if(!ModuleExists("Other.Mod"))
@wrapMethod(PlayerPuppet)
protected cb func OnGameAttached() -> Bool {
  Log("I'm in a wrapper");
  wrappedMethod();
}
```

Only a small subset of REDscript is allowed in the condition of the @if annotation. You can only use:

* Boolean constants (`true`, `false`)
* Standard boolean operators (`&&`, `||`, `!`, `?:`)
* The `ModuleExists` function which determines whether the given module exists at compile time
