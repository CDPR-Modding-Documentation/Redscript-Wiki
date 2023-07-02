---
description: Introduction to modules.
---

# Modules

You can organize your REDscript code into modules, which are separate files that start with a line like this:

```haskell
module MyMod.Utils
```

The module name can consist of any number of identifiers joined by dots. Each module has its own namespace, which means that you can only use the names you define in it inside that module, unless you make them public and import them in another file. This also prevents name clashes with other scripts in the game, so you can reuse any name you want.

If you do not include a module header at the beginning of your file, your code will be in the global scope and implicitly visible everywhere.

Here is an example of a module:

```swift
module Math.Constants

public func Pi() -> Float = 3.14159
public func E() -> Float = 2.71828
```

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
