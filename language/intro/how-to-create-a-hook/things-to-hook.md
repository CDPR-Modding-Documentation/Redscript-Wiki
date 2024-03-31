# Things to hook

Summary

Published: Mar 31 2024 by [manavortex](https://app.gitbook.com/u/NfZBoxGegfUqB33J9HXuCs6PVaC3 "mention")\
Last documented edit: Mar 31 2024 by [manavortex](https://app.gitbook.com/u/NfZBoxGegfUqB33J9HXuCs6PVaC3 "mention")

This page lists things that you can hook into (straight from the code of Addicted, thanks **Roms1383** for the contribution!)

If you want to see an example, check [.](./ "mention") -> [#example](./#example "mention")

### Player Actions

After the player is spawned:

```swift
@wrapMethod(PlayerPuppet)
protected cb func OnMakePlayerVisibleAfterSpawn(evt: ref<EndGracePeriodAfterSpawn>) -> Bool { }
```

When a status effect is applied or removed

```swift
@wrapMethod(PlayerPuppet)
protected cb func OnStatusEffectApplied(evt: ref<ApplyStatusEffectEvent>) -> Bool
protected cb func OnStatusEffectRemoved(evt: ref<RemoveStatusEffect>) -> Bool
```

When the player does something

```swift
@wrapMethod(PlayerPuppet)
protected cb func OnAction(action: ListenerAction, consumer: ListenerActionConsumer) -> Bool
```

When the player interacts with an item

```swift
@wrapMethod(ItemActionsHelper)
public final static func ProcessItemAction(gi: GameInstance, executor: wref<GameObject>, itemData: wref<gameItemData>, actionID: TweakDBID, fromInventory: Bool) -> Bool
public final static func ProcessItemAction(gi: GameInstance, executor: wref<GameObject>, itemData: wref<gameItemData>, actionID: TweakDBID, fromInventory: Bool, quantity: Int32) -> Bool
```

When the player consumes a quickslot item

```swift
@wrapMethod(ItemActionsHelper)
public final static func ConsumeItem(executor: wref<GameObject>, itemID: ItemID, fromInventory: Bool) -> Void
```

When the player consumes an item from backpack

```swift
@wrapMethod(ItemActionsHelper)
public final static func PerformItemAction(executor: wref<GameObject>, itemID: ItemID) -> Void
```

When the player equips/unequips an item

```swift
@wrapMethod(EquipmentSystemPlayerData)
private final func UnequipItem(itemID: ItemID) -> Void
private final func UnequipItem(equipAreaIndex: Int32, opt slotIndex: Int32, opt forceRemove: Bool) -> Void
```

When the player equips Cyberware at a Ripperdoc

```swift
@wrapMethod(RipperDocGameController)
private final func EquipCyberware(itemData: wref<gameItemData>) -> Bool 
```

### Different menus

In the character creator

```swift
@wrapMethod(ConsumeAction)
protected func ProcessStatusEffects(const actionEffects: script_ref<array<wref<ObjectActionEffect_Record>>>, gameInstance: GameInstance) -> Void
```

On equipment preview

```swift
@wrapMethod(inkPuppetPreviewGameController)
protected cb func OnPreviewInitialized() -> Bool
```

Photo mode

```swift
@wrapMethod(PhotoModePlayerEntityComponent)
private final func SetupInventory(isCurrentPlayerObjectCustomizable: Bool) 
```

ESC menu

```swift
@wrapMethod(SingleplayerMenuGameController)
protected cb func OnInitialize() -> Bool
```

Weapon selection wheel (?)

```swift
@wrapMethod(DpadWheelGameController)
protected cb func OnInitialize() -> Bool
```

When receiving a message

```swift
@wrapMethod(GenericMessageNotification)
protected cb func OnInitialize() -> Bool
```

???

```swift
@wrapMethod(GenericMessageNotification)
protected cb func OnHandlePressInput(evt: ref<inkPointerEvent>) -> Bool 
```

???

```swift
@wrapMethod(PopupsManager)
protected cb func OnPlayerAttach(playerPuppet: ref<GameObject>) -> Bool
```

When showing/hiding a custom popup

```swift
@addMethod(PopupsManager)
protected cb func OnShowCustomPopup(evt: ref<ShowCustomPopupEvent>) -> Bool 
protected cb func OnHideCustomPopup(evt: ref<HideCustomPopupEvent>) -> Bool 
```

### BackpackMainGameController

```swift
@wrapMethod(BackpackMainGameController)
protected cb func OnInitialize() -> Bool
protected cb func OnItemDisplayClick(evt: ref<ItemDisplayClickEvent>) -> Bool 
private final func NewShowItemHints(itemData: wref<UIInventoryItem>) 
```

### CraftingGarmentItemPreviewGameController

```swift
@wrapMethod(CraftingGarmentItemPreviewGameController)
protected cb func OnCrafrtingPreview(evt: ref<CraftingItemPreviewEvent>) -> Bool
```

### EquipmentSystemPlayerData

```swift
@wrapMethod(EquipmentSystemPlayerData)
public final func OnAttach()
public func LockVisualChanges() 
public func UnlockVisualChanges() 
public final const func IsVisualSetActive() -> Bool 
public final const func IsSlotOverriden(area: gamedataEquipmentArea) -> Bool 
private final const func ShouldUnderwearBeVisibleInSet() -> Bool
private final const func ShouldUnderwearTopBeVisibleInSet() -> Bool 
public final func OnRestored()
public final func OnQuestDisableWardrobeSetRequest(request: ref<QuestDisableWardrobeSetRequest>) 
public final func OnQuestRestoreWardrobeSetRequest(request: ref<QuestRestoreWardrobeSetRequest>) 
public final func OnQuestEnableWardrobeSetRequest(request: ref<QuestEnableWardrobeSetRequest>)
public final func EquipWardrobeSet(setID: gameWardrobeClothingSetIndex)
public final func QuestHideSlot(area: gamedataEquipmentArea)
public final func QuestHideSlot(area: gamedataEquipmentArea) 
public final func QuestRestoreSlot(area: gamedataEquipmentArea)
private final func ClearItemAppearanceEvent(area: gamedataEquipmentArea)
private final func ResetItemAppearanceEvent(area: gamedataEquipmentArea) 
private final func ResetItemAppearance(area: gamedataEquipmentArea, opt force: Bool)
```

### gameuiInGameMenuGameController

```swift
@wrapMethod(gameuiInGameMenuGameController)
protected cb func OnInitialize() -> Bool 
protected cb func OnPuppetReady(sceneName: CName, puppet: ref<gamePuppet>) -> Bool 
protected cb func OnEquipmentChanged(value: Variant) -> Bool 
protected cb func OnUninitialize() -> Bool
```

### gameuiInventoryGameController

```swift
@wrapMethod(gameuiInventoryGameController)
protected cb func OnInitialize() -> Bool
protected cb func OnUninitialize() -> Bool
private final func SetupSetButton() -> Void 
protected cb func OnWardrobeBtnClick(evt: ref<inkPointerEvent>) -> Bool 
protected cb func OnWardrobePopupClose(data: ref<inkGameNotificationData>)
protected cb func OnBack(userData: ref<IScriptable>) -> Bool
protected cb func OnEquipmentClick(evt: ref<ItemDisplayClickEvent>) -> Bool 
```

### gameuiPhotoModeMenuController

```swift
@wrapMethod(gameuiPhotoModeMenuController)
protected cb func OnInitialize() -> Bool
protected cb func OnAddMenuItem(label: String, attribute: Uint32, page: Uint32) -> Bool 
protected cb func OnShow(reversedUI: Bool) -> Bool
protected cb func OnSetAttributeOptionEnabled(attribute: Uint32, enabled: Bool) -> Bool
```

### PhotoModeMenuListItem

```swift
@wrapMethod(PhotoModeMenuListItem)
private final func StartArrowClickedEffect(widget: inkWidgetRef) 
```

### inkInventoryPuppetPreviewGameController

```swift
@wrapMethod(inkInventoryPuppetPreviewGameController)
protected cb func OnInitialize() -> Bool 
```

### InventoryItemDisplayController

```swift
@wrapMethod(InventoryItemDisplayController)
public func Bind(inventoryDataManager: ref<InventoryDataManagerV2>, equipmentArea: gamedataEquipmentArea, opt slotIndex: Int32, opt displayContext: ItemDisplayContext, opt setWardrobeOutfit: Bool, opt wardrobeOutfitIndex: Int32) {
public func Bind(inventoryScriptableSystem: ref<UIInventoryScriptableSystem>, equipmentArea: gamedataEquipmentArea, opt slotIndex: Int32, displayContext: ItemDisplayContext)
protected func RefreshUI() 
protected func NewUpdateRequirements(itemData: ref<UIInventoryItem>)
```

### InventoryItemModeLogicController

```swift
@wrapMethod(InventoryItemModeLogicController)
private final func UpdateOutfitWardrobe(active: Bool, activeSetOverride: Int32) 
protected cb func OnItemDisplayClick(evt: ref<ItemDisplayClickEvent>) -> Bool 
protected cb func OnItemDisplayHoverOver(evt: ref<ItemDisplayHoverOverEvent>) -> Bool
private final func SetInventoryItemButtonHintsHoverOver(const displayingData: script_ref<InventoryItemData>, opt display: ref<InventoryItemDisplayController>)
private final func HandleItemClick(const itemData: script_ref<InventoryItemData>, actionName: ref<inkActionName>, opt displayContext: ItemDisplayContext, opt isPlayerLocked: Bool)  
```

### PhotoModePlayerEntityComponent

```swift
@wrapMethod(PhotoModePlayerEntityComponent)
private final func OnGameAttach()
private final func SetupInventory(isCurrentPlayerObjectCustomizable: Bool) 
protected cb func OnItemAddedToSlot(evt: ref<ItemAddedToSlot>) -> Bool

```

### QuestTrackerGameController

```swift
@wrapMethod(QuestTrackerGameController)
protected cb func OnInitialize() -> Bool
```

### UIInventoryItem

```swift
@wrapMethod(UIInventoryItem)
public final func IsEquipped(opt force: Bool) -> Bool 
public final func IsTransmogItem() -> Bool 
public final static func Make(player: wref<PlayerPuppet>, transactionSystem: ref<TransactionSystem>, uiScriptableSystem: wref<UIScriptableSystem>) -> ref<UIInventoryItemsManager> 
public final func IsItemEquippedInSlot(itemID: ItemID, slotID: TweakDBID) -> Bool
public final func IsItemTransmog(itemID: ItemID) -> Bool 
```

### WardrobeSetPreviewGameController

```swift
@wrapMethod(WardrobeSetPreviewGameController)
protected cb func OnInitialize() -> Bool
protected cb func OnPreviewInitialized() -> Bool 
public final func RestorePuppetEquipment() 

```

### ComputerInkGameController (already wrapped by Virtual Atelier)

Check for it with this annotation:

```redscript
@if(ModuleExists("VirtualAtelier.Site"))
@if(!ModuleExists("VirtualAtelier.Site"))
```

```swift
@wrapMethod(ComputerInkGameController)
private final func ShowMenuByName(elementName: String) -> Void 
private final func HideMenuByName(elementName: String) -> Void 
```

### ComputerControllerPS  (already wrapped by Virtual Atelier)

```swift

@wrapMethod(ComputerControllerPS)
public final func GetMenuButtonWidgets() -> array<SComputerMenuButtonWidgetPackage>
```

### ComputerMenuButtonController  (already wrapped by Virtual Atelier)

```swift

@wrapMethod(ComputerMenuButtonController)
public func Initialize(gameController: ref<ComputerInkGameController>, widgetData: SComputerMenuButtonWidgetPackage) -> Void
```

### BrowserController (already wrapped by Virtual Atelier)

```swift
@wrapMethod(BrowserController)
protected cb func OnPageSpawned(widget: ref<inkWidget>, userData: ref<IScriptable>) -> Bool 
```

### Consumables

When the consumable cooldown is over:

```swift
@wrapMethod(ConsumableTransitions) 
protected final func ChangeConsumableAnimFeature(stateContext: ref, scriptInterface: ref, newState: Bool) -> Void {}
```

When the player heals themselves

```swift
@wrapMethod(UseHealChargeAction)
protected func ProcessStatusEffects(const actionEffects: script_ref<array<wref<ObjectActionEffect_Record>>>, gameInstance: GameInstance) -> Void
```

When the player uses a consumable

```swift
@wrapMethod(ConsumeAction)
protected func ProcessStatusEffects(const actionEffects: script_ref<array<wref<ObjectActionEffect_Record>>>, gameInstance: GameInstance) -> Void
```

### Other stuff

When you skip time

```swift
@wrapMethod(TimeskipGameController)
private final func Apply() -> Void
```
