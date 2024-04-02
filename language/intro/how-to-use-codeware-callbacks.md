---
description: How we can use events for control flow
---

# How to use Codeware callbacks

Unlike [how-to-create-a-hook](how-to-create-a-hook/ "mention"), a callback can be be registered **through Codeware** **callback system**.&#x20;

{% hint style="info" %}
This method depends on Codeware. You can't use it alone.
{% endhint %}

## Example

```swift
class ExampleEnv extends ScriptableEnv {
  // instance variables
  private let callbackSystem: wref<CallbackSystem>;
  private let found: Bool = false;
  
  // This function is always executed
  private func OnLoad() {
    this.callbackSystem = GameInstance.GetCallbackSystem();
    this.callbackSystem.RegisterCallback(n"Input/Key", this, n"OnKeyInput", true);
    
    LogChannel(n"DEBUG", s"ExampleEnv loaded");
  } 
  
  // Will always listen, but only react once
  private cb func OnKeyInput(event: ref<KeyInputEvent>) {
    if this.found {
      return;
    }    
    LogChannel(n"DEBUG", s"You pressed your key, going dormant now: \(event.GetAction()) \(event.GetKey())");
    this.found = true;
  }

}
```

Let's go over it bit by bit:

### OnLoad

This function is always executed, because our `ExampleEnv` inherits from `ScriptableEnv`.

* It sets the instance variable `callbackSystem`, making sure that we don't have to get it each time we want to use it.
* It **binds to the `Input/Key` callback**, telling Codeware to run the function `OnKeyInput` each time
* It prints to log

### OnKeyInput

{% hint style="info" %}
You can see that this function is a callback from the `cb func` rather than just `func`&#x20;
{% endhint %}

This function will be executed every time the registered event (`Input/Key`) is triggered.

Since we are checking our internal variable `found` before running any logic, this will only become active once.

## Unregistering callbacks

Once you're done with your logic, you can unregister your callback again.

{% hint style="danger" %}
As of **v0.5.19** (Mar 31 2024), unregistering `Input/Key` will crash the game (link to [github issue](https://github.com/jac3km4/redscript/issues/104)). Unregistering other kinds of callback works, though.
{% endhint %}

```swift
private cb func OnKeyInput(event: ref<KeyInputEvent>) {
  LogChannel(n"DEBUG", s"Input: \(event.GetAction()) \(event.GetKey())");
  this.callbackSystem.UnregisterCallback(n"Input/Key", this, n"OnKeyInput");
}
```
