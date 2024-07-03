# Sleeping and Skipping Time

## Differentiating Between Sleeping and Skip Time

Sleeping in a bed and pressing "Skip Time" from the Hub Menu both utilize the Time Skip Popup, and fire the same events when completed. This can make it challenging to differentiate between whether or not the player has just finished "waiting", or if they slept in a bed.

One way to detect this difference reliably is to catch the player's input on the Hub Menu. If the player has clicked on the "Skip Time" button from this menu, then we know that they decided to wait instead of sleeping in a bed. We can store whether or not this occurred and then clear it as soon as the Time Skip event is complete.

The UI sends many `inkPointerEvent`s to menus, so it's important to filter these events by whether or not the action was a `click`.

These examples use a Scriptable System and Codeware's `GetGameInstance()` for convenience; however, you could also manage this data on any class that you prefer, including base game scripts using `@addField()`.

{% code fullWidth="true" %}
```swift
module MySleepingSystemModule

// Catch pressing the Skip Time button from the Hub Menu.
@wrapMethod(HubTimeSkipController)
protected cb func OnTimeSkipButtonPressed(e: ref<inkPointerEvent>) -> Bool {
	if e.IsAction(n"click") {
		MySleepingSystem.GetInstance(GetGameInstance()).SetSkippingTimeFromHubMenu(true);
	}
	
	return wrappedMethod(e);
}

// When the Time Skip Popup is initialized, check if the Skipping Time from Hub Menu flag 
// was just set. If so, we are not sleeping. This step is necessary because the Skipping Time 
// from Hub Menu flag is transient, and will be cleared as soon as the Time Skip Popup is 
// uninitialized (see below).
@wrapMethod(TimeskipGameController)
protected cb func OnInitialize() -> Bool {
	let SleepingSystem = MySleepingSystem.GetInstance(GetGameInstance());
	if Equals(SleepingSystem.GetSkippingTimeFromHubMenu(), true) {
		SleepingSystem.SetWasSleeping(false);
	} else {
		SleepingSystem.SetWasSleeping(true);
	}
	
	return wrappedMethod();
}

// Event that fires regardless of the Time Skip outcome (Finished, cancelled, etc).
// Always clear the "Skipping Time from Hub Menu" flag when this happens.
@wrapMethod(TimeskipGameController)
protected cb func OnUninitialize() -> Bool {
	MySleepingSystem.GetInstance(GetGameInstance()).SetSkippingTimeFromHubMenu(false);
	
	return wrappedMethod();
}

// Event that fires after successfully sleeping or skipping time. (Player didn't cancel out of the menu)
@wrapMethod(TimeskipGameController)
protected cb func OnCloseAfterFinishing(proxy: ref<inkAnimProxy>) -> Bool {
	MySleepingSystem.GetInstance(GetGameInstance()).OnTimeSkipFinished();
	
	return wrappedMethod(proxy);
}

// Example class that we're using to handle this gameplay event.
public final class MySleepingSystem extends ScriptableSystem {
	private let skippingTimeFromHubMenu: Bool = false;
	private let wasSleeping: Bool = false;

	public final static func GetInstance(gameInstance: GameInstance) -> ref<MySleepingSystem> {
		let instance: ref<MySleepingSystem> = GameInstance.GetScriptableSystemsContainer(gameInstance).Get(n"MySleepingSystemModule.MySleepingSystem") as MySleepingSystem;
		return instance;
	}
	
	public final func SetSkippingTimeFromHubMenu(value: Bool) -> Void {
		this.skippingTimeFromHubMenu = value;
	}
	
	public final func GetSkippingTimeFromHubMenu() -> Bool {
		return this.skippingTimeFromHubMenu;
	}
	
	public final func SetWasSleeping(value: Bool) {
		this.wasSleeping = value;
	}
	
	public final func OnTimeSkipFinished() -> Void {
		if this.wasSleeping {
			FTLog("The player was sleeping in a bed! How cozy.");
			// Do something cool when the player sleeps in a bed.
		} else {
			FTLog("The player was NOT sleeping. They Skipped Time from the Menu.");
			// Do something cool when the player skips time.
		}
	}
}
```
{% endcode %}
