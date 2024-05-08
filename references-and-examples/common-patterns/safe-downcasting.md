# Safe downcasting

The `as` operator returns null when a dynamic cast fails. You can use it combined with `IsDefined` to perform safe downcasts.

```swift
let manager = employee as Manager;
if IsDefined(manager) {
  // employee is known to be Manager here
}
```
