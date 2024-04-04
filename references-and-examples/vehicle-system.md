---
description: Cool stuff you can do with vehicles
---

# Vehicle system



### React to headlight changes

```swift
native class vehicleChangeHeadLightModeEvent extends Event {}

@addMethod(VehicleObject)
protected cb func OnChangeHeadLightModeEvent(evt: ref<vehicleChangeHeadLightModeEvent>) {
    if this.IsPlayerMounted() {
        LogChannel(n"DEBUG", s"OnChangeHeadLightModeEvent \(this.GetVehiclePS().GetVehicleControllerPS().GetHeadLightMode())");
    }
}
```
