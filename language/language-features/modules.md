---
description: Introduction to modules.
---

# Modules

Every REDscript file can optionally start with a module header:

```haskell
module MyMod.Utils
```

The module name can be any chain of identifiers separated by dots. All the definitions in the module will be placed in it's own namespace and they won't be visible from other modules unless they're both public and explicitly imported. They also won't conflict with other names in the game, so you can even re-use the same names.

If you do not include a module header all your definitions will be placed in the global scope and made visible everywhere.

Here's an example module:

```swift
module Math.Constants

public func Pi() -> Float = 3.14159
public func E() -> Float = 2.71828
```

This module can then be used in other files:

```swift
// Some codemodule MyMod
import Math.Constants.Pi

class Circle {
  let circumference: Float;

  public final func Radius() -> Float {
    return this.circumference / (2.0 * Pi());
  }
}
```

There are also other ways to import symbols from a module:

* `import Math.Constants.*` will import all public symbols (`Pi` and `E` in this case).
* `import Math.Constants.{Pi, E}` will explicitly import symbols `Pi` and `E`.
