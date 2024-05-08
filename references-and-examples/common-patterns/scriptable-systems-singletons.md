# Scriptable systems (singletons)

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
