# Persistence

The fields of classes that extend `ScriptableSystem` or `PersistentState` (e.g. `ScriptedPuppetPS`) can be declared with the **persistent** modifier to be persisted in game saves.

You can persist data of all types except for `String`, `Variant` and `ResRef` (and arrays of these types).

Instances of classes can be persisted too, but note that their fields must also be marked as **persistent**, or they won't be persisted and instead they'll be initialized with defaults.

```swift
module MyMod

public class Entry {
    // both of these will be persisted
    public persistent let related: TweakDBID;
    public persistent let lasting: Int32;
    // this won't be persisted
    public let temporary: Int32;
}

public class MySystem extends ScriptableSystem {
    private persistent let m_entries: array<ref<Entry>>;
}
```
