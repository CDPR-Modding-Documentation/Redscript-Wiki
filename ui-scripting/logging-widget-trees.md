# Logging Widget Trees

## Summary

**Published**: Mar 31 2024 by [manavortex](https://app.gitbook.com/u/NfZBoxGegfUqB33J9HXuCs6PVaC3 "mention")\
**Last documented update**: Apr 15 2024 by [manavortex](https://app.gitbook.com/u/NfZBoxGegfUqB33J9HXuCs6PVaC3 "mention")

This page will tell you how to log widget trees and layer trees.

### Wait, that's not what I want!

To learn more about logging, check [logging.md](../language/logging.md "mention")

## Logging layer trees

You can find a list of existing UI layers on [basics.md](basics.md "mention") -> [#inksystemlayers](basics.md#inksystemlayers "mention").

To traverse a layer's children, you first need to get the corresponding layer's virtual window:

```swift
private static func PrintLayerHierarchy(CName layerName) {
    let window = GameInstance.GetInkSystem().GetLayer(layerName).GetVirtualWindow();
    let rootWidget = window.GetWidgetByPathName(n"Root") as inkCanvas;
    // The function LogWidgetTree is defined below
    LogChannelTree(n"DEBUG", baseHudRoot);
}

PrintLayerHierarchy(n"inkHUDLayer")
```

### Logging Widget Trees

{% hint style="info" %}
Big thanks to **Rayshader** for walking me through this with the patience of a saint!
{% endhint %}

You can add this code to your [`logs.reds`](../language/logging.md) file:

<pre class="language-swift"><code class="lang-swift"><strong>public static func LogWidgetTree(channel: CName, widget: wref&#x3C;inkCompoundWidget>, opt props: Bool, opt indent: String) {
</strong>    let i = 0;

    if StrLen(indent) == 0 {
      LogChannel(channel, s"\(widget.GetName()) - \(widget.GetClassName())");
    }
    while i &#x3C; widget.GetNumChildren() {
      let child: wref&#x3C;inkWidget> = widget.GetWidget(i);
      let compChild = child as inkCompoundWidget;

      LogChannel(channel, s"\(indent)|-- \(child.GetName()) - \(child.GetClassName())");
      if props {
        let anchor = child.GetAnchor();
        let size = child.GetSize();
        let scale = child.GetScale();

        LogChannel(channel, s"\(indent){");
        LogChannel(channel, s"\(indent)  anchor: inkEAnchor.\(anchor)");
        LogChannel(channel, s"\(indent)  size: (\(size.X), \(size.Y))");
        LogChannel(channel, s"\(indent)  scale: (\(scale.X), \(scale.Y))");
        let wText = child as inkText;

        if IsDefined(wText) {
          LogChannel(channel, s"\(indent)  text: \"\(wText.GetText())\"");
          LogChannel(channel, s"\(indent)  fontSize: \"\(wText.GetFontSize())\"");
        }
        LogChannel(channel, s"\(indent)}");
      }
      if IsDefined(compChild) {
        LogChannelTree(channel, compChild, props, s"\(indent)    ");
      }
      i += 1;
    }
  }
</code></pre>
