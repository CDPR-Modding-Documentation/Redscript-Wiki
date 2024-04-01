# Logging

### Logging and tracing functions

{% hint style="info" %}
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

Since game version 2.01, some of these declarations are not imported by default like previously. You must declare them yourself if you want to use them, otherwise you'll get an `UNRESOLVED_FN` error at compilation time.&#x20;

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
