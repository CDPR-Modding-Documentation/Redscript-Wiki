---
description: How to do stuff with weapons, the Redscript way
---

# Weapons

### Weapon damage debugging

{% hint style="info" %}
You can find a list of weapon damage effects next door on the [yellow wiki.](https://app.gitbook.com/s/4gzcGtLrr90pVjAWVdTc/for-mod-creators-theory/references-lists-and-overviews/cheat-sheet-tweak-ids/weapons/cheat-sheet-weapon-damage-effects)
{% endhint %}

To see damage types and damage proccs, you can useAngeVil's script ([gist](https://gist.github.com/Berdagon/6ae42e4ff6b0964808a91d32951c52e4)) and copy it to e.g. [`Cyberpunk 2077`](https://app.gitbook.com/s/4gzcGtLrr90pVjAWVdTc/for-mod-users/users-modding-cyberpunk-2077/the-cyberpunk-2077-game-directory)`/r6/scripts/debug_damage_types.reds`:

{% hint style="warning" %}
You have to include the [logging.md](logging.md "mention") functions for this to work!
{% endhint %}

{% embed url="https://gist.github.com/Berdagon/6ae42e4ff6b0964808a91d32951c52e4" %}

### Hiding (Parts of) a component when a weapon is equipped

<pre class="language-swift"><code class="lang-swift">module TutorialHidingComponentPartsModule

private func hideComponent(componentName: String) -> Void {
  let player = GetPlayer(GetGameInstance());

  let component: ref&#x3C;IComponent> = player.FindComponentByName(t(componentName))
  // or cast the component to a different type 
  // by using "as entSkinnedMeshComponent" / changing e.g. ref&#x3C;entSkinnedMeshComponent>
  if IsDefined(component) {
    component.Toggle(false);
  }
}

<strong>
</strong>// If you don't know the component name, you can iterate over the components
private func showComponent(componentName: String) -> Void {
  let player = GetPlayer(GetGameInstance());
  let components = player.GetComponents();

  for component in components {
    if StrContains(s"\(component.GetName())", componentName) {
      component.Toggle(true);
    }
  }
}

@wrapMethod(PlayerPuppet)
protected cb func OnWeaponEquipEvent(event: ref&#x3C;WeaponEquipEvent>) -> Bool {
  let weaponRecord: wref&#x3C;ItemObject> = event.item;

  if !IsDefined(weaponRecord) {
    return wrappedMethod(event);
  }
  let id = weaponRecord.GetItemID();
  let id_str = TDBID.ToStringDEBUG(ItemID.GetTDBID(id));

  if StrContains(id_str, "Items.your_weapon") {
    hideComponent("your_component_name");
  }
  wrappedMethod(event);
}

private func OnItemUnequipped(slot: TweakDBID, item: ItemID) -> Void {

 if StrContains(TDBID.ToStringDEBUG(ItemID.GetTDBID(item)), "Items.your_weapon") {
    showComponent("your_component_name");
  }
}

private class PlayerPuppetAttachmentSlotsCallbackVenuzdnor extends PlayerPuppetAttachmentSlotsCallback {

  public let m_player: wref&#x3C;PlayerPuppet>;

  public func OnItemUnequipped(slot: TweakDBID, item: ItemID) -> Void {
    OnItemUnequipped(slot, item);
  }
}

@wrapMethod(EquipmentSystemPlayerData)
func OnRestored() -> Void {
  if IsDefined(this.m_owner as PlayerPuppet) {
    let attachmentSlotCallback: ref&#x3C;PlayerPuppetAttachmentSlotsCallback> = new PlayerPuppetAttachmentSlotsCallbackVenuzdnor();
    attachmentSlotCallback.m_player = (this.m_owner as PlayerPuppet);
    attachmentSlotCallback.slotID = t"AttachmentSlots.WeaponRight";
    GameInstance.GetTransactionSystem((this.m_owner as PlayerPuppet).GetGame()).RegisterAttachmentSlotListener(this.m_owner, attachmentSlotCallback);
  }
  wrappedMethod();
}

</code></pre>

