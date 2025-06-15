---
description: >-
  This guide will show you how you can declare a generic callback thanks to
  Codeware's reflection.
---

# Generic callbacks

## Requirements

You need Codeware ([Nexus](https://www.nexusmods.com/cyberpunk2077/mods/7780) | [Wiki](https://github.com/psiberx/cp2077-codeware/wiki#reflection)) to use the following classes. It provides a Reflection module which is exactly what we need to make our callback in a generic way.

## How to

We will declare two classes: `Callback` to use with methods of a class, and `StaticCallback` to use with static methods of a class.

### Callback

#### Declaration

```swift
public class Callback extends DelayCallback {
  private let m_target: wref<IScriptable>;
  private let m_fn: CName;
  private let m_data: array<Variant>;

  public static func Create(target: wref<IScriptable>,
                            fn: CName,
                            opt data: array<Variant>) -> ref<Callback> {
    let self = new Callback();

    self.m_target = target;
    self.m_fn = fn;
    self.m_data = data;
    return self;
  }

  public func Call() {
    if !IsDefined(this.m_target) {
      return;
    }
    Reflection.GetClassOf(ToVariant(this.m_target))
              .GetFunction(this.m_fn)
              .Call(this.m_target, this.m_data);
  }
}
```

We declare `Callback` which inherits [DelayCallback](https://nativedb.red4ext.com/DelayCallback). This way we can use `Callback` if we need it as-is, and we can also use it with [DelaySystem](https://nativedb.red4ext.com/DelaySystem).

We implement the method `Call()` which is expected by DelaySystem / DelayCallback. If we want to use it without DelaySystem, we simply need to call `Call()` when we want to execute a callback by ourself.

#### Example

```swift
public class MyService extends ScriptableService {

  // ...

  public cb func OnSessionReady(event: ref<GameSessionEvent>) {
    if event.IsPreGame() {
      return;
    }
    let delaySystem = GameInstance.GetDelaySystem(GetGameInstance());
    let callback = Callback.Create(this, n"OnPlay", [42]);
    let delay: Float = 1.0; // seconds

    delaySystem.DelayCallback(callback, delay, false);
    // callback.Call();
  }

  // [cb] is required to make function a callback.
  // [arg] is an optional argument which can be passed when calling [Callback.Create].
  private cb func OnPlay(arg: Int32) {
    FTLog(s"arg: \(arg)");
    // do stuff after [delay] s.
  }

}
```

### StaticCallback

#### Declaration

```swift
public class StaticCallback extends DelayCallback {
  private let m_fn: CName;
  private let m_data: array<Variant>;

  public static func Create(fn: CName, opt data: array<Variant>) -> ref<StaticCallback> {
    let self = new StaticCallback();

    self.m_fn = fn;
    self.m_data = data;
    return self;
  }

  public func Call() {
    Reflection.GetGlobalFunction(this.m_fn)
              .Call(this.m_data);
  }
}
```

This looks like `Callback`, we still inherits `DelayCallback` to be compatible with `DelaySystem`. But this time we only need a function's name.

{% hint style="warning" %}
Static methods of a class are registered as global functions. You'll need to use this syntax: `n"ModuleName.ClassName::MethodName"`. Method must be declared with keyword `cb`. `ModuleName` is optional.
{% endhint %}

#### Example

This shows how it looks like when declaring in a custom module `MyModule`. It is not mandatory and you can declare it in the global scope.

```swift
module MyModule

public class MyService extends ScriptableService {

  // ...

  public cb func OnSessionReady(event: ref<GameSessionEvent>) {
    if event.IsPreGame() {
      return;
    }
    let delaySystem = GameInstance.GetDelaySystem(GetGameInstance());
    let callback = StaticCallback.Create(n"MyModule.MyService::OnPlay", [n"Choom"]);
    let delay: Float = 1.0; // seconds

    delaySystem.DelayCallback(callback, delay, false);
    // callback.Call();
  }

  // [cb] is required to make function a callback.
  // [name] is an optional argument which can be passed when calling [StaticCallback.Create].
  private static cb func OnPlay(name: CName) {
    FTLog(s"name: \(name)");
    // do stuff after [delay] s.
  }

}
```
