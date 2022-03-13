# String formatting

### String interpolation

REDscript supports string interpolation through a special string literal syntax with the 's' prefix. You can use it to format strings with arbitrary expressions.

```swift
Log(s"My name is \(name) and I am \(year - birthYear) years old");
```

The code above gets desugared into:

```swift
Log("My name is " + name + " and I am " + ToString(year - birthYear) + " years old");
```

In the generated code, all types except `String` get converted to `String` via `ToString`.&#x20;

### String addition overloads

The game scripts come with a set of operator overloads that allow you to append values of a number of types to strings, which can be used for basic string formatting.

```swift
// string addition is defined Int32, Float and several other types 
Log("My name is " + name + " and I am " + (year - birthYear) + " years old");
```

