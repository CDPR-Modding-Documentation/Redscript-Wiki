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

### Persisting data

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

### **Constructing Class Object**

Redscript does not have Class constructors and to set the data of class fields you would have to either do that manually

```swift
public class CustomClass{
    //all the variables 
    public let myInt: Int32;
    public let myString: String;
}
```

```swift
let myCustomClass = new CustomClass();
myCustomClass.myInt = 1;
myCustomClass.myString = "Hello";
```

&#x20;or if you want to do it with a single clean line

```swift
public class CustomClass{
    //all the variables 
    public let myInt: Int32;
    public let myString: String;
    
    public static func Create(inputInt: Int32, inputString: String) -> ref<CustomClass>{
        //you create the new instance of your class here instead of in your code
        //and set its variables
        let self = new CustomClass();
        self.myInt = inputInt;
        self.myString = inputString;
        return self;
    }
}
```

and how to use it

```swift
let myCustomClass = CustomClass.Create(1,"Hello");
```

### DelaySystem and DelayCallback

DelaySystem is a class that allows async call of DelayCallback after the set amount of time has passed.

this is how to create a DelayCallback

```swift
public class CustomCallback extends DelayCallback{
    //all the data that your Call function needs
    private let myInt: Int32;
    
    public func Call() {
        //custom function that i want to be called when the specified time has passed
        if this.myInt>1 
        {
            LogChannel(n"DEBUG", "Input is bigger than 1");
        }
        else
        {
            LogChannel(n"DEBUG", "Input is smaller than 1");
        }
    }
    
    public static func Create(inputInt: Int32) -> ref<CustomCallback> {
        //use this way to create your Callback class in one line
        let self = new CustomCallback();
        self.myInt = inputInt;
        return self;
    }
}
```

and this is how you can use the DelaySystem to call the Callback function with a delay

```swift
let delaySystem = GameInstance.GetDelaySystem(this.m_player.GetGame());
let inputInt: Int32 = 2;
let delay: Float = 1.0;
let isAffectedByTimeDilation: Bool = false;
delaySystem.DelayCallback(CustomCallback.Create(inputInt), delay, isAffectedByTimeDilation);
```
