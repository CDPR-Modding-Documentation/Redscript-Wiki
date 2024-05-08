# Hash maps

There are several hash map implementations available in the game. Most importantly, `inkHashMap` (`Uint64 -> ref<IScriptable>`) and `inkWeakHashMap` (`Uint64 -> wref<IScriptable>`). All custom classes extend `IScriptable` therefore they can be used as values in those hash maps.

```swift
let map = new inkHashMap();
hashMap.Insert(TDBID.ToNumber(t"key1"), MyClass.Create(1));
hashMap.Insert(TDBID.ToNumber(t"key2"), MyClass.Create(2));
let value: ref<MyClass> = map.Get(TDBID.ToNumber(t"key1")) as MyClass;
```
