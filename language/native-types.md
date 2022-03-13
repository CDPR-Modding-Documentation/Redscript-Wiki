---
description: Built-in types available in the game runtime.
---

# Built-in Types

## Logical Types

| Keyword | Type               | Values          |
| :-----: | ------------------ | --------------- |
|  `Bool` | Boolean logic type | `true`  `false` |

## Integer Types

|  Keyword | Type                    | Range                                                       |
| :------: | ----------------------- | ----------------------------------------------------------- |
|  `Int8`  | 8-bit Signed Integer    | `-128` to `127`                                             |
|  `Uint8` | 8-bit Unsigned Integer  | `0` to `255`                                                |
|  `Int16` | 16-bit Signed Integer   | `-32,768` to `32,767`                                       |
| `Uint16` | 16-bit Unsigned Integer | `0` to `65,535`                                             |
|  `Int32` | 32-bit Signed Integer   | `−2,147,483,648` to `2,147,483,647`                         |
| `Uint32` | 32-bit Unsigned Integer | `0` to `4,294,967,295`                                      |
|  `Int64` | 64-bit Signed Integer   | `−9,223,372,036,854,775,808` to `9,223,372,036,854,775,807` |
| `Uint64` | 64-bit Unsigned Integer | `0` to `18,446,744,073,709,551,615`                         |

## Floating-Point Types

| Keyword  | Type                    | Range                                                                                                                  |
| -------- | ----------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `Float`  | 32-bit Single-Precision | 6-9 significant decimal digits ([more info](https://en.wikipedia.org/wiki/Single-precision\_floating-point\_format))   |
| `Double` | 64-bit Double-Precision | 15-17 significant decimal digits ([more info](https://en.wikipedia.org/wiki/Double-precision\_floating-point\_format)) |

## Literal Types

| Keyword     | Type                        | Prefix | Example                            |
| ----------- | --------------------------- | ------ | ---------------------------------- |
| `String`    | Mutable character string    |        | `"Hello world"`                    |
| `CName`     | Non-mutable string constant | `n`    | `n"VehicleComponent"`              |
| `ResRef`    | Resource reference path     | `r`    | `r"base\\anim_cooked.cookedanims"` |
| `TweakDBID` | TweakDB Record ID           | `t`    | `t"Items.RequiredItemStats"`       |

`String` values are stored internally as a null-terminated character array, unfortunately the bytecode doesn't support accessing the individual characters as an array.

`CName` values are stored in-engine as a [64-bit hash key](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo\_hash\_function#FNV-1a\_hash) to a interned string pool. Class, function and field names are stored in the CName pool, so any methods that need a dynamic reference a scripted component will use a `CName` value.

`ResRef` values are similar to `CName` values, except they specifically refer to archive resource files and presumably use a separate optimized string pool. Unlike `CName`, `ResRef` doesn't have any defined operators.

`TweakDBID` is used as the primary key for all `*_Record` types stored in TweakDB, the engine's internal database.

## Other Types

|  Keyword  | Type                                         |
| :-------: | -------------------------------------------- |
| `Variant` | A dynamic type that can store any other type |

## Operators

The RED4 scripting runtime implements most operators as native functions. Only the equals `==` and not equals `!=` operators are implemented in bytecode. The **redscript** compiler provides a number of operator symbols as shorthand.

This table lists the available operators (in precedence block order) and what types support them.

| Type                  | Symbol | Logical | Integer | Float | String | CName | TweakDBID |
| --------------------- | :----: | :-----: | :-----: | :---: | :----: | :---: | :-------: |
| Negate                |   `-`  |    -    |    ✓    |   ✓   |    -   |   -   |     -     |
| Logical Not           |   `!`  |    ✓    |    -    |   -   |    -   |   -   |     ✓¹    |
| Bitwise Not           |   `~`  |    -    |    ✓    |   -   |    -   |   -   |     -     |
|                       |        |         |         |       |        |       |           |
| Multiplication        |   `*`  |    -    |    ✓    |   ✓   |   ✓²   |   -   |     -     |
| Division              |   `/`  |    -    |    ✓    |   ✓   |    -   |   -   |     -     |
| Modulo                |   `%`  |    -    |    ✓    |   ✓   |    -   |   -   |     -     |
|                       |        |         |         |       |        |       |           |
| Addition              |   `+`  |    -    |    ✓    |   ✓   |   ✓³   |   ✓⁴  |     ✓⁴    |
| Subtraction           |   `-`  |    -    |    ✓    |   ✓   |    -   |   -   |     -     |
|                       |        |         |         |       |        |       |           |
| Less Than             |   `<`  |    -    |    ✓    |   ✓   |    -   |   -   |     -     |
| Less Than or Equal    |  `<=`  |    -    |    ✓    |   ✓   |    -   |   -   |     -     |
| Greater Than          |   `>`  |    -    |    ✓    |   ✓   |    -   |   -   |     -     |
| Greater Than or Equal |  `>=`  |    -    |    ✓    |   ✓   |    -   |   -   |     -     |
|                       |        |         |         |       |        |       |           |
| Equals                |  `==`  |    ✓    |    ✓    |   ✓   |    ✓   |   ✓⁵  |     ✓     |
| Not Equals            |  `!=`  |    ✓    |    ✓    |   ✓   |    ✓   |   ✓⁵  |     ✓     |
|                       |        |         |         |       |        |       |           |
| Logical And           |  `&&`  |    ✓    |    -    |   -   |    -   |   -   |     -     |
| Logical Or            | `\|\|` |    ✓    |    -    |   -   |    -   |   -   |     -     |
| Bitwise And           |   `&`  |    -    |    ✓    |   -   |    -   |   -   |     -     |
| Bitwise Or            |  `\|`  |    -    |    ✓    |   -   |    -   |   -   |     -     |
| Bitwise Xor           |   `^`  |    -    |    ✓    |   -   |    -   |   -   |     -     |
|                       |        |         |         |       |        |       |           |
| Assign                |   `=`  |    ✓    |    ✓    |   ✓   |    ✓   |   ✓   |     ✓     |
| Assign Add            |  `+=`  |    -    |    ✓    |   ✓   |    ✓   |   -   |     ✓     |
| Assign Subtract       |  `-=`  |    -    |    ✓    |   ✓   |    -   |   -   |     -     |
| Assign Multiply       |  `*=`  |    -    |    ✓    |   ✓   |    -   |   -   |     -     |
| Assign Divide         |  `/=`  |    -    |    ✓    |   ✓   |    -   |   -   |     -     |
| Assign Bitwise And    |  `&=`  |    -    |    ✓    |   -   |    -   |   -   |     -     |
| Assign Bitwise Or     |  `\|=` |    -    |    ✓    |   -   |    -   |   -   |     -     |

### Notes

1. The Logical Not `!` operator for `TweakDBID` is overridden to return `!TDBID.IsValid(a)`
2.  Strings can be multiplied by an `Int32` to repeat the string:

    ```swift
    public static func OperatorMultiply(a: String, count: Int32) -> String
    ```
3. `String` addition is [concatenation](https://en.wikipedia.org/wiki/Concatenation). There are native functions to allow most types can be concatenated with `String`
4. `CName` and `TweakDBID` addition is [concatenation](https://en.wikipedia.org/wiki/Concatenation) (and presumably involves some kind of internal lookup to a known value)
5. The **redscript** compiler doesn't currently support the `==` and `!=` symbols for the `CName` type, use the `Equals(a,b)` and `NotEquals(a,b)` intrinsic functions for now.
