# Heterogeneous array literals

If you define a function to accept an array of `Variant`:

```swift
func AcceptVariants(variants: array<Variant>) {}
```

Then that function can be called with an array literal containing elements of different types:

```swift
// all elements get implicitly converted to Variant
AcceptVariants([1, new Vector2(1, 2), new inkText()]);
// you can achieve the same thing with an explicitly typed local
let variants: array<Variant> = [1, new Vector2(1, 2), new inkText()];
```
