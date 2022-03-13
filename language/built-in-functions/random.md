# Random

### Random functions

```swift
native func RandF() -> Float
native func RandRangeF(min: Float, max: Float) -> Float
native func RandNoiseF(seed: Int32, max: Float, opt min: Float) -> Float
native func RandRange(min: Int32, max: Int32) -> Int32
native func RandDifferent(lastValue: Int32, range: Int32) -> Int32
native func CalcSeed(object: ref<IScriptable>) -> Int32
```
