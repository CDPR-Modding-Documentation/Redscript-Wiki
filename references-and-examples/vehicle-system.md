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



### Dynamically removing components

Requires Codeware

```
class MyVehicleWatcher extends ScriptableSystem {
  private let m_callbackSystem: wref<CallbackSystem>;

  private func OnAttach() {
    this.m_callbackSystem = GameInstance.GetCallbackSystem();
    this.m_callbackSystem.RegisterCallback(n"Entity/Assemble", this, n"OnEntityAssemble");
  }

  private cb func OnEntityAssemble(event: ref<EntityLifecycleEvent>) {
    // Something useful to note is that during vehicle assembly the vehicle record is null, so you cannot use it to identify the car. This is why entity appearance is used 
    
    let entity = event.GetEntity();

    if entity.GetTemplatePath() == r"base\\vehicles\\sport\\v_sport2_quadra_type66__basic_01.ent"
    && Equals(entity.GetCurrentAppearanceName(), n"quadra_type66_vtech_noralee") {

      let vehicle = entity as VehicleObject;
      if !IsDefined(vehicle) {
        return;
      }

      let components = vehicle.GetComponents();

      for comp in components {
        if Equals(comp.GetName(), n"body_01") {
          let meshComp = comp as MeshComponent;
          meshComp.chunkMask = 0ul;
        }
      }
    }
  }
}
```
