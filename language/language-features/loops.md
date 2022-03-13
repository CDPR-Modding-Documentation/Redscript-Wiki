# Loops

REDscript supports two kinds of loops: `while` and `for..in` loops.

### while

The `while` statement works the same way it does in most languages. It allows you to execute a block of code multiple times based on a conditional expression.

```swift
let i = 0;
let array = ["a", "b", "c"];

while i < ArraySize(array) {
  let elem = array[i];
  Log(elem);
  i += 1;
}
// will log a, b and c
```

### for..in

The `for..in` loop allows you to iterate over all elements of an array.

```swift
let array = ["a", "b", "c"];

for elem in array {
  Log(elem);
}
// will log a, b and c
```
