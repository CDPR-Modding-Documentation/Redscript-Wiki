# Logging

### Logging and tracing functions

```swift
native func Log(const text: script_ref<String>) -> Void
native func LogWarning(const text: script_ref<String>) -> Void
native func LogError(const text: script_ref<String>) -> Void
native func LogChannel(channel: CName, const text: script_ref<String>)
native func LogChannelWarning(channel: CName, const text: script_ref<String>) -> Void
native func LogChannelError(channel: CName, const text: script_ref<String>) -> Void

native func FTLog(const value: script_ref<String>) -> Void
native func FTLogWarning(const value: script_ref<String>) -> Void
native func FTLogError(const value: script_ref<String>) -> Void

native func Trace() -> Void
native func TraceToString() -> String
```

Since v2.01, these declarations are not imported by default like previously. You must declare them yourself if you want to use them, otherwise you'll get an `UNRESOLVED_FN` error at compilation time. You can create a `Logs.reds` file and copy/paste the content above. Copy the file in `r6\scripts\`. Now all scripts (including yours) will have access to these functions.
