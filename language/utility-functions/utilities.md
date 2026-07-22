# Utilities

### Enum

```swift
native func EnumGetMax(type: CName) -> Int64
native func EnumGetMin(type: CName) -> Int64

native func EnumValueToString(const enumStr: script_ref<String>, enumValue: Int64) -> String
native func EnumValueFromString(const enumStr: script_ref<String>, const enumValue: script_ref<String>) -> Int64

native func EnumValueToName(enumName: CName, enumValue: Int64) -> CName
native func EnumValueFromName(enumName: CName, enumValue: CName) -> Int64
```

### Localization

```swift
native func LocKeyToString(hashKey: CName) -> String
native func GetLocalizedText(const textKey: script_ref<String>) -> String
native func GetLocalizedTextGanderDepened(const textKey: script_ref<String>, variantIsFemale: Bool) -> String
native func GetLocalizedTextGanderDepenedByKey(hashKey: CName, variantIsFemale: Bool) -> String
native func GetLocalizedTextByKey(hashKey: CName) -> String
native func GetLocalizedItemNameByString(hashKey: CName) -> String
native func GetLocalizedItemNameByCName(hashKey: CName) -> String
```

### Target search

```swift
native func TSF_All(mask: TSFMV) -> TargetSearchFilter
native func TSF_Any(mask: TSFMV) -> TargetSearchFilter
native func TSF_Not(mask: TSFMV) -> TargetSearchFilter
native func TSF_And(tsf1: TargetSearchFilter, tsf2: TargetSearchFilter, opt tsf3: TargetSearchFilter, opt tsf4: TargetSearchFilter) -> TargetSearchFilter
native func TSF_Or(tsf1: TargetSearchFilter, tsf2: TargetSearchFilter, opt tsf3: TargetSearchFilter, opt tsf4: TargetSearchFilter) -> TargetSearchFilter
```
