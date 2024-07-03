---
description: >-
  This guide describes how you can write logs while debugging your scripts. It
  can be helpful too to get feedback from players when they find bugs.
---

# Logging

### Logging and tracing functions

{% hint style="danger" %}
With Cyberpunk >= 2.01, you need to paste the snippet below into e.g. \
`r6\scripts\Logs.reds`\
if you want to avoid `UNRESOLVED_FN` errors at compile time.
{% endhint %}

This will give access to these functions to all scripts (including yours):&#x20;

```swift
native func Log(const text: script_ref<String>) -> Void
native func LogWarning(const text: script_ref<String>) -> Void
native func LogError(const text: script_ref<String>) -> Void

// output goes to CET window
native func LogChannel(channel: CName, const text: script_ref<String>)
native func LogChannelWarning(channel: CName, const text: script_ref<String>) -> Void
native func LogChannelError(channel: CName, const text: script_ref<String>) -> Void

native func FTLog(const value: script_ref<String>) -> Void
native func FTLogWarning(const value: script_ref<String>) -> Void
native func FTLogError(const value: script_ref<String>) -> Void

native func Trace() -> Void
native func TraceToString() -> String
```

{% hint style="warning" %}
Do not package this file with your mod though because that would lead to conflicts with other scripts.
{% endhint %}

## Example usage

```swift
@wrapMethod(EquipmentSystemPlayerData)
func OnRestored() -> Void {
    wrappedMethod(); // call the original function
    LogChannel(n"DEBUG", "Player Initialized"); // create a log output
}
```

## Where does it go?

You can see the output on your [CET console](https://app.gitbook.com/s/-MP5jWcLZLbbbzO-\_ua1-887967055/console/console), or in the `scripting.log` inside the CET folder.

## Using Codeware

You can use the function `ModLog` to create logs with your mod's name as a prefix. It will make it easier for you to go over your logs and filter them:

```swift
module MyMod.Logger

public static func Log(value: script_ref<String>) -> Void {
  ModLog(n"MyMod", value);
}
```

You can use it like this:

```swift
import Codeware.*
import MyMod.Logger.*

class CrazyService extends ScriptableService {
  private cb func OnLoad() {
    Log("[CrazyService] OnLoad");
  }
}

// It will log something like:
// ... [MyMod] [CrazyService] OnLoad
```

We can even improve on this. Add a parameter to directly prefix our logs with the current class we are logging from:

```swift
module MyMod.Logger

public static func Log(object: wref<IScriptable>,
                       value: script_ref<String>) -> Void {
  ModLog(n"MyMod", s"[\(object.GetClassName())] \(value)");
}
```

Usage will be:

```swift
import Codeware.*
import MyMod.Logger.*

class CrazyService extends ScriptableService {
  private cb func OnLoad() {
    Log(this, "OnLoad");
  }
}

// It will log something like:
// ... [MyMod] [CrazyService] OnLoad
```

This output is the same except you don't need to write the class name yourself when calling function `Log`. This is convenient as you don't have to worry when you copy/paste this line in another function. The class name will be deduced for you using `this` parameter.

You could also add another parameter to add the function name as a prefix too. However you'll have to manually write it. It cannot be deduced automatically.
