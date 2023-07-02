# Loops

REDscript supports two types of loops: `while` and `for..in`.

### while

You can use the `while` statement to repeatedly execute a block of code as long as a condition is true. This is similar to how it works in many other languages. For example, you can use a while loop to print each element of an array:

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

You can use the `for..in` statement to iterate through all the elements of an array one by one. It's often cleaner than using an explicit `while` loop:

```swift
let array = ["a", "b", "c"];

for elem in array {
  Log(elem);
}
// will log a, b and c
```
