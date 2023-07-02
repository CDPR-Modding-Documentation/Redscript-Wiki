# String formatting

### String interpolation

REDscript lets you insert any expression into a string using the `s` prefix. This is a convenient way to create formatted strings without using concatenation or conversions.

```swift
Log(s"My name is \(name) and I am \(year - birthYear) years old");
```

This is equivalent to:

```swift
Log("My name is " + name + " and I am " + ToString(year - birthYear) + " years old");
```

The compiler automatically converts expressions of any type other than `String` using `ToString`.

### String addition overloads

The game scripts let you use the `+` operator to join strings with values of different types, such as `Int32` or `Float`. This is a simple way to format strings without using conversion functions.

```swift
// You can use addition to combine strings with integers, floats and several other types 
Log("My name is " + name + " and I am " + (year - birthYear) + " years old");
```

