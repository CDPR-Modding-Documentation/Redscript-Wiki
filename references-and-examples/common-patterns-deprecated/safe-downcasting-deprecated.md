---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Safe downcasting

The `as` operator returns `null` when a dynamic cast fails. You can use it combined with `IsDefined` to perform safe downcasts. The following example shows how you can safe downcast a [VehicleObject](https://nativedb.red4ext.com/VehicleObject) to a [WheeledObject](https://nativedb.red4ext.com/WheeledObject), a [CarObject](https://nativedb.red4ext.com/CarObject) or a [BikeObject](https://nativedb.red4ext.com/BikeObject):

```swift
let vehicle: ref<VehicleObject>;

let wheels = vehicle as WheeledObject;
if IsDefined(wheels) {
  // vehicle is known to be a WheeledObject.
}

let car = vehicle as CarObject;
if IsDefined(car) {
  // vehicle is known to be a CarObject.
}

let bike = vehicle as BikeObject;
if IsDefined(bike) {
  // vehicle is known to be a BikeObject.
}
```

{% hint style="info" %}
`as` operator will keep the same kind of reference, that is `ref<T>` to `ref<U>` and `wref<T>` to `wref<U>`.
{% endhint %}
