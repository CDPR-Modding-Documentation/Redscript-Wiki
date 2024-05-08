---
description: Influencing Cyberpunk's UI with REDscript
---

# UI Scripting

## Summary

**Published**: Mar 31 2024 by [manavortex](https://app.gitbook.com/u/NfZBoxGegfUqB33J9HXuCs6PVaC3 "mention")\
**Last documented update**: Mar 31 2024 by [manavortex](https://app.gitbook.com/u/NfZBoxGegfUqB33J9HXuCs6PVaC3 "mention")

This page will tell you how to script the UI (turning widgets on and off).&#x20;

### Wait, this is not what I want!

* For help on [logging-widget-trees.md](logging-widget-trees.md "mention"), check the corresponding wiki page!
* You can find guides on how to add custom UI elements on [Codeware's github repo](https://github.com/psiberx/cp2077-codeware/wiki#layers-and-windows).

## General information

All different kinds of UI are loaded at the same time. Components don't get added or removed, but simply rendered inactive (hidden).&#x20;

{% hint style="info" %}
The code snippets on this page are function calls which do nothing on their own; you need to add them to your class and call them from your internal logic.
{% endhint %}

## InkSystemLayers

You can access any existing layer via the game instance's InkSystem. Simply pass the layer's name as `CName` (for a list of existing options, see the table below):

```swift
GameInstance.GetInkSystem().GetLayer(n"inkHUDLayer")
```

### Querying existing layers

```swift
private static func DumpInkHudLayers() {
  let inkSystem = GameInstance.GetInkSystem();
  let layers = inkSystem.GetLayers();

  for layer in layers {
    LogChannel(n"DEBUG", s"UI Layer: \(layer.GetLayerName()) \(layer.GetGameController().GetClassName())");
  }
}
```

### List of existing layers (as of 2.12\_a)

| Layer name (as CName)         | Class                            |                   |
| ----------------------------- | -------------------------------- | ----------------- |
| `inkHUDLayer`                 | `gameuiRootHudGameController`    | the generic hud   |
| `inkWatermarksLayer`          |                                  |                   |
| `inkWaitingSignLayer`         |                                  |                   |
| `inkSystemNotificationsLayer` |                                  |                   |
| `inkLoadingLayer`             |                                  |                   |
| `inkGameNotificationsLayer`   | `gameuiPopupsManager`            |                   |
| `inkMenuLayer`                | `gameuiInGameMenuGameController` | ingame menu (ESC) |
| `inkVideoLayer`               | `gameuiHUDVideoPlayerController` |                   |
| `inkWorldLayer`               |                                  |                   |
| `inkOffscreenLayer`           |                                  |                   |
| `inkAdvertis`ementsLayer      |                                  |                   |
| `inkStreetSignsLayer`         |                                  |                   |
| `inkPhotoModeLayer`           | `gameuiWidgetGameController`     | photo mode HUD    |

## Accessing layers

To find functions for traversing widget trees, check the [nativeDB page for inkCompoundWidget](https://nativedb.red4ext.com/inkCompoundWidget).

{% hint style="info" %}
Naturally, CDPR did not give their view elements unique names. Check the [#example](./#example "mention") at the end of the page for how to find and enable a certain widget.
{% endhint %}

If there are multiple widgets with the same name, the call below can return any of them.

```swift
let window = GameInstance.GetInkSystem().GetLayer(n"inkHUDLayer").GetVirtualWindow();
let root = window.GetWidgetByPathName(n"Root") as inkCanvas;

// Those two are the same:
let hudMiddle1 = root.GetWidgetByPathName(n"HUDMiddleWidget");
let hudMiddle2 = window.GetWidgetByPathName(n"Root/HUDMiddleWidget");
```

## Example

{% hint style="info" %}
This example only contains the scripting logic. For how to **trigger** the code, check either [how-to-create-a-hook](../../language/intro/how-to-create-a-hook/ "mention") or [codeware-callbacks.md](../codeware-callbacks.md "mention").
{% endhint %}

```swift
  private let fpsWidget: wref<inkWidget>;
  
  // Since CDPR couldn't be arsed to give unique names to HUD stuff, we need to iterate to find the widget we want.
  // This function checks the prerequisites.
  public func InitWidget() {
    // Don't run this if we already have our widget
    if (IsDefined(this.fpsWidget)) {
      return;
    }
    
    let window = GameInstance.GetInkSystem().GetLayer(n"inkHUDLayer").GetVirtualWindow();
    let rootWidget = window.GetWidgetByPathName(n"Root") as inkCanvas;

    // Put this into a variable in case you end up adding layers in your loop
    let numChildren = rootWidget.GetNumChildren();

    let i = 0;
    while (i < numChildren && i < 100) {
      let child = rootWidget.GetWidgetByIndex(i);
      // Since Redscript doesn't support continue, keep this in a function for early retrurns
      this.CheckWidget(child as inkCompoundWidget);
      i = i + 1;
    }
  }
  
  private func CheckWidget(widget: wref<inkCompoundWidget>) {

    // The widget we're looking for is nested like this (2.12_a):
    // ----------------------------------------------------------------------
    //  |-- HUDMiddleWidget - inkCanvasWidget
    //      |-- Root - inkCanvasWidget
    //          |-- inkRectangleWidget5 - inkRectangleWidget
    //          |-- inkHorizontalPanelWidget5 - inkHorizontalPanelWidget
    //              |-- fpsText - inkTextWidget
    //              |-- fpsCounter - inkTextWidget

    if (!Equals(n"HUDMiddleWidget", widget.GetName())) {
      return;
    }

    let rootWidget = widget.GetWidgetByPathName(n"Root") as inkCanvas;

    if (!IsDefined(rootWidget)) { return; }

    let horizontalWidget = rootWidget.GetWidgetByPathName(n"inkHorizontalPanelWidget5") as inkHorizontalPanel;

    if (!IsDefined(horizontalWidget)) { return; }

    let fpsTextWidget = horizontalWidget.GetWidgetByPathName(n"fpsText") as inkText;

    if (!IsDefined(fpsTextWidget)) { return; }

    widget.SetVisible(true);
    rootWidget.SetVisible(true);
    horizontalWidget.SetVisible(true);
    fpsTextWidget.SetVisible(true);
    
    this.fpsWidget = widget;

    let fpsCounterWidget = horizontalWidget.GetWidgetByPathName(n"fpsCounter") as inkText;
    if (!IsDefined(fpsCounterWidget)) { return; }

    fpsCounterWidget.SetVisible(true);
  }
```
