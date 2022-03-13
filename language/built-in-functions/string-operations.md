# String operations

### Conversions

```swift
native func NameToString(n: CName) -> String
native func StringToName(const str: script_ref<String>) -> CName

native func FloatToStringPrec(value: Float, precision: Int32) -> String
native func StringToFloat(const value: script_ref<String>, opt defValue: Float) -> Float

native func StringToInt(const value: script_ref<String>, opt defValue: Int32) -> Int32
native func StringToUint64(const value: script_ref<String>, opt defValue: Uint64) -> Uint64
native func StringToHex(const str: script_ref<String>, lineLength: Uint32) -> String
```

### String operations

```swift
native func StrCmp(const str: script_ref<String>, const with: script_ref<String>, opt length: Int32, opt noCase: Bool) -> Int32
func StrContains(const str: script_ref<String>, const subStr: script_ref<String>) -> Bool
native func StrBeginsWith(const str: script_ref<String>, const match: script_ref<String>) -> Bool
native func StrEndsWith(const str: script_ref<String>, const match: script_ref<String>) -> Bool

native func StrLen(const str: script_ref<String>) -> Int32
native func StrChar(i: Int32) -> String
native func StrUpper(const str: script_ref<String>) -> String
native func StrLower(const str: script_ref<String>) -> String
native func StrFindFirst(const str: script_ref<String>, const match: script_ref<String>) -> Int32
native func StrFindLast(const str: script_ref<String>, const match: script_ref<String>) -> Int32
native func StrReplace(const str: script_ref<String>, const match: script_ref<String>, const with: script_ref<String>) -> String
native func StrReplaceAll(const str: script_ref<String>, const match: script_ref<String>, const with: script_ref<String>) -> String
native func StrSplit(const str: script_ref<String>, const divider: script_ref<String>, opt includeEmpty: Bool) -> array<String>
native func StrSplitFirst(const str: script_ref<String>, const divider: script_ref<String>, out left: String, out right: String) -> Bool
native func StrSplitLast(const str: script_ref<String>, const divider: script_ref<String>, out left: String, out right: String) -> Bool
native func StrBeforeFirst(const str: script_ref<String>, const match: script_ref<String>) -> String
native func StrAfterFirst(const str: script_ref<String>, const match: script_ref<String>) -> String
native func StrBeforeLast(const str: script_ref<String>, const match: script_ref<String>) -> String
native func StrAfterLast(const str: script_ref<String>, const match: script_ref<String>) -> String
native func StrMid(const str: script_ref<String>, first: Int32, opt length: Int32) -> String
native func StrLeft(const str: script_ref<String>, length: Int32) -> String
native func StrRight(const str: script_ref<String>, length: Int32) -> String
func StrUpperFirst(const str: script_ref<String>, opt lenght: Int32) -> String

native func IsStringValid(const n: script_ref<String>) -> Bool
native func IsNameValid(n: CName) -> Bool
native func IsStringNumber(const value: script_ref<String>) -> Bool

native func UnicodeStringEqual(const str: script_ref<String>, const str2: script_ref<String>) -> Bool
native func UnicodeStringCompare(const str: script_ref<String>, const str2: script_ref<String>) -> Int32
native func UnicodeStringLessThan(const str: script_ref<String>, const str2: script_ref<String>) -> Bool
native func UnicodeStringLessThanEqual(const str: script_ref<String>, const str2: script_ref<String>) -> Bool
```
