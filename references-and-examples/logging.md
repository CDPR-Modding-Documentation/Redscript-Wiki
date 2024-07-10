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

## Improve logging

You can use the function `FTLog` to create logs with your mod's name as a prefix. It will make it easier for you to go over your logs and filter them:

```swift
module MyMod.Logger

public static func MyLog(value: script_ref<String>) -> Void {
  FTLog(s"[MyMod] \(value)");
}

public static func MyLogWarn(value: script_ref<String>) -> Void {
  FTLogWarning(s"[MyMod] \(value)");
}

public static func MyLogError(value: script_ref<String>) -> Void {
  FTLogError(s"[MyMod] \(value)");
}
```

You can use it like this:

```swift
import Codeware.*
import MyMod.Logger.*

class CrazyService extends ScriptableService {
  private cb func OnLoad() {
    MyLog("Psycho stuff!");
    MyLogWarn("Psycho stuff!");
    MyLogError("Psycho stuff!");
  }
}

// It will log something like:
// ... [MyMod] Psycho stuff!
// ... [MyMod] Psycho stuff!      // Colored in Game Log
// ... [MyMod] Psycho stuff!      // Colored in Game Log
```

We can even improve on this with [Codeware](https://github.com/psiberx/cp2077-codeware/wiki). It provides the function [GetStackTrace](https://github.com/psiberx/cp2077-codeware/blob/main/scripts/Scripting/StackTrace.reds#L7) so we can deduce the class and function where our logging function is executed:

```swift
module MyMod.Logger

public static func MyLog(value: script_ref<String>) -> Void {
  let entries = GetStackTrace(1, true);
  let entry = entries[0];
  let trace = "";

  if IsDefined(entry.object) {
    trace = s"[\(entry.class)][\(entry.function)]";
  } else {
    trace = s"[\(entry.function)]";
  }
  FTLog(s"[MyMod] \(trace) \(value)");
}

// Same with MyLogWarn / MyLogError and FTLogWarning / FTLogError.
```

You can use it the same way, this time it will log something like:

```swift
// ... [MyMod] [CrazyService][OnLoad] Psycho stuff!
```

This is convenient as you don't have to worry when you copy/paste this line in another function. The class and function name will be deduced for you. It also works when using a global function.

{% hint style="info" %}
You can improve this function to list more entries of the stack trace. This is up to you to implement it if you want. An other alternative worth checking is to use [redscript-dap](https://github.com/jac3km4/redscript-dap), a debugger which integrates with VS Code.
{% endhint %}
