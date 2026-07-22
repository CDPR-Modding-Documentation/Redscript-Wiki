# Math

### Integer operations

```swift
native func Max(a: Int32, b: Int32) -> Int32
native func Min(a: Int32, b: Int32) -> Int32
native func Abs(a: Int32) -> Int32
native func Clamp(v: Int32, min: Int32, max: Int32) -> Int32
func IsInRange(value: Int32, a: Int32, b: Int32) -> Bool
```

### Float operations

```swift
native func MaxF(a: Float, b: Float) -> Float
native func MinF(a: Float, b: Float) -> Float
native func AbsF(a: Float) -> Float
native func ClampF(v: Float, min: Float, max: Float) -> Float
native func RoundF(a: Float) -> Int32
func RoundMath(f: Float) -> Int32
func RoundTo(f: Float, decimal: Int32) -> Float
func IsInRange(value: Float, a: Float, b: Float) -> Bool

native func PowF(a: Float, x: Float) -> Float
native func LogF(a: Float) -> Float
native func ExpF(a: Float) -> Float
native func SqrtF(a: Float) -> Float
native func CeilF(a: Float) -> Int32
native func FloorF(a: Float) -> Int32
native func SinF(a: Float) -> Float
native func AsinF(a: Float) -> Float
native func CosF(a: Float) -> Float
native func AcosF(a: Float) -> Float
native func TanF(a: Float) -> Float
native func AtanF(a: Float, b: Float) -> Float
native func SqrF(a: Float) -> Float
native func LerpF(alpha: Float, a: Float, b: Float, opt clamp: Bool) -> Float

func Pi() -> Float
native func AngleNormalize(a: Float) -> Float
native func AngleDistance(target: Float, current: Float) -> Float
native func AngleApproach(target: Float, cur: Float, step: Float) -> Float
func LerpAngleF(alpha: Float, a: Float, b: Float) -> Float
native func Deg2Rad(deg: Float) -> Float
native func Rad2Deg(rad: Float) -> Float
```
