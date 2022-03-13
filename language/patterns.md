# Patterns

### Debug logs

Messages logged to the DEBUG channel are visible in the CET console by default (without having to enable game logs).

```swift
LogChannel(n"DEBUG", "something")
```

### General purpose hash map

There are several hash map implementations available in the game. Most importantly, `inkHashMap` (`Uint64 -> ref<IScriptable>`) and `inkWeakHashMap` (`Uint64 -> wref<IScriptable>`). All custom classes extend `IScriptable` therefore they can be used as values in those hash maps.

```swift
let map = new inkHashMap();
hashMap.Insert(TDBID.ToNumber(t"key1"), MyClass.Create(1));
hashMap.Insert(TDBID.ToNumber(t"key2"), MyClass.Create(2));
let value: ref<MyClass> = map.Get(TDBID.ToNumber(t"key1")) as MyClass;
```

### Safe downcasting

The `as` operator returns null when a dynamic cast fails. You can use it combined with `IsDefined` to perform safe downcasts.

```swift
let manager = employee as Manager;
if IsDefined(manager) {
  // employee is known to be Manager here
}
```

### Heterogeneous array literals

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

### Custom scriptable system (singleton pattern)

You can define a custom scriptable system:

```swift
module MyMod

public class MySystem extends ScriptableSystem {
  private func OnAttach() -> Void {
    LogChannel(n"DEBUG", "MySystem::OnAttach");
  }

  private func OnDetach() -> Void {
    LogChannel(n"DEBUG", "MySystem::OnDetach");
  }

  public func GetData() -> Float {
    return GetPlayer(this.GetGameInstance()).GetGunshotRange();
  }
}
```

This scriptable system can then be accessed using the scriptable system container:

```swift
let container = GameInstance.GetScriptableSystemsContainer(gameInstance);
// don't forget the namespace if you're using modules
let system = container.Get(n"MyMod.MySystem") as MySystem;

LogChannel(n"DEBUG", ToString(system.GetData()));
```

