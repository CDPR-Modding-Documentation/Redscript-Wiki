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

Requires Codeware ([Nexus](https://www.nexusmods.com/cyberpunk2077/mods/7780) | [GitHub](https://github.com/psiberx/cp2077-codeware))

```swift
class MyVehicleWatcher extends ScriptableSystem {
  private let m_callbackSystem: wref<CallbackSystem>;

  private func OnAttach() {
    this.m_callbackSystem = GameInstance.GetCallbackSystem();
    this.m_callbackSystem.RegisterCallback(n"Entity/Assemble", this, n"OnEntityAssemble");
  }

  private cb func OnEntityAssemble(event: ref<EntityLifecycleEvent>) {
    // Something useful to note is that during vehicle assembly the vehicle record
    // is null, so you cannot use it to identify the car. This is why entity 
    // appearance is used.
    let entity = event.GetEntity();
    let templatePath = r"base\\vehicles\\sport\\v_sport2_quadra_type66__basic_01.ent";
    let appearanceName = n"quadra_type66_vtech_noralee";

    if entity.GetTemplatePath() == templatePath && Equals(entity.GetCurrentAppearanceName(), appearanceName) {
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
