---
description: Introduction to modules.
---

# Modules

You can organize your REDscript code into modules, which are separate files that start with a line like this:

```haskell
module MyMod.Utils
```

A module name is a series of identifiers separated by dots. Each module has its own namespace and its items are not visible outside of the module unless you declare them as public and import them in another file. This helps you structure your code and avoids name conflicts with existing game scripts, so you can use any name without overriding in-game definitions.

If you do not include a module header at the beginning of your file, your code will be in the global scope and implicitly visible everywhere.

Here is an example of a module:

```swift
module Math.Constants

public func Pi() -> Float = 3.14159
public func E() -> Float = 2.71828
```

{% hint style="info" %}
Types and functions placed in modules are only accessible by fully qualified names at runtime. This matters when you try to

* interact with them from CET or red4ext
* refer to them with a CName, which is accepted by some methods like `ScriptableSystemsContainer.Get`

The fully qualified name consists of the full module path followed by the name of the type/function. In this case these two functions become `Math.Constants.Pi` and `Math.Constants.E`.&#x20;

One should also be careful not to define existing in-game natives like `LogChannel` inside a module because that will always lead to a name mismatch.
{% endhint %}

This module has two public functions that return math constants. You can use them in another file by importing them and calling them by name like this:

```swift
// In another file
import Math.Constants.Pi

class Circle {
  let circumference: Float;

  public final func Radius() -> Float {
    return this.circumference / (2.0 * Pi());
  }
}
```

There are different ways to import symbols from a module:

* `import Math.Constants.*` will import all public symbols (both `Pi` and `E` in this case)
* `import Math.Constants.{Pi, E}` will only import the symbols you specify (`Pi` and `E` in this case)
