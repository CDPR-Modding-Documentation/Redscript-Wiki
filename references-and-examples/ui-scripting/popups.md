---
description: Scriptable popups
---

# Popups

The game has various built-in popup widgets that can be spawned easily.

1. **SimpleScreenMessage**

````swift
```
    let warningMsg: SimpleScreenMessage;
    warningMsg.isShown = true;
    warningMsg.duration = 5.00;
    warningMsg.message = "Hello, this is a simple message";
    warningMsg.type = SimpleMessageType.Relic;
    /*
    enum SimpleMessageType {
        Undefined = 0,
        Negative = 1,
        Neutral = 2,
        Vehicle = 3,
        Apartment = 4,
        Relic = 5,
        Money = 6,
        Reveal = 7,
        Boss = 8,
        Twintone = 9,
        Police = 10,
      }
      */
    GameInstance.GetBlackboardSystem(this.GetGame()).Get(GetAllBlackboardDefs().UI_Notifications).SetVariant(GetAllBlackboardDefs().UI_Notifications.WarningMessage, ToVariant(warningMsg), true);
 
```
````



2. **GenericMessageNotification**

Use this if you want a quick popup with a button

Example usage: [https://github.com/psiberx/cp2077-equipment-ex/blob/master/scripts/UI/ConflictsPopup.reds](https://github.com/psiberx/cp2077-equipment-ex/blob/master/scripts/UI/ConflictsPopup.reds)'



### **Using Codeware**

Codeware provides primitives for a lot of common UI components including pop ups.

Here's a simple pop up implemented using Codeware's `InGamePopup`with custom text&#x20;

{% hint style="info" %}
Remember to add this import:

`import Codeware.UI.*`
{% endhint %}

````swift
```
public class MinimalPopup extends InGamePopup {
    protected let m_header: ref<InGamePopupHeader>;
    protected let m_footer: ref<InGamePopupFooter>;
    protected let m_content: ref<InGamePopupContent>;
    protected let m_text: wref<inkText>;
    protected let customText: String = "Default text";
    
    public static func Create() -> ref<MinimalPopup> {
        let self: ref<MinimalPopup> = new MinimalPopup();
        self.CreateInstance();
        return self;
    }

    protected cb func OnInitialize() {
        super.OnInitialize();
        this.m_text.SetText(this.customText);
    }
    
    protected cb func OnCreate() {
	super.OnCreate();
	this.m_container.SetSize(new Vector2(800.0, 600.0));
	this.m_header = InGamePopupHeader.Create();
	this.m_header.SetTitle("Minimal Popup");
	this.m_header.SetFluffRight("Text Example");
	this.m_header.Reparent(this);
	this.m_footer = InGamePopupFooter.Create();
	this.m_footer.SetFluffIcon(n"fluff_triangle2");
	this.m_footer.SetFluffText("Simple popup with updatable text");
	this.m_footer.Reparent(this);
	this.m_content = InGamePopupContent.Create();
	this.m_content.Reparent(this);

	let canvas = new inkCanvas();
	canvas.SetName(n"canvas");
	canvas.SetAnchor(inkEAnchor.Fill);
	canvas.Reparent(this.m_content.GetRootCompoundWidget());

	let text = new inkText();
	text.SetName(n"updatableText");
	text.SetFontFamily("base\\gameplay\\gui\\fonts\\raj\\raj.inkfontfamily");
	text.SetFontStyle(n"Medium");
	text.SetFontSize(30);
	text.SetHorizontalAlignment(textHorizontalAlignment.Center);
	text.SetVerticalAlignment(textVerticalAlignment.Center);
	text.SetAnchor(inkEAnchor.Centered);
	text.SetAnchorPoint(new Vector2(0.5, 0.5));
	text.SetMargin(new inkMargin(0.0, 0.0, 0.0, 0.0));
	text.SetWrapping(true, 700.0);
	text.SetFitToContent(true);
	text.SetStyle(r"base\\gameplay\\gui\\common\\main_colors.inkstyle");
	text.BindProperty(n"tintColor", n"MainColors.Blue");
        text.SetText("Default text");
        text.Reparent(canvas);
	this.m_text = text;
    }

    public func SetCustomText(text: String) -> Void {
        this.customText = text;
    }
} 
```
````

Which can then be used like this:

````swift
```
let controller = GameInstance.GetInkSystem().GetLayer(n"inkHUDLayer").GetGameController() as inkGameController;
let popup = MinimalPopup.Create();
popup.SetCustomText(text);
popup.Open(controller);
```
````

See [Codeware Wiki](https://github.com/psiberx/cp2077-codeware/wiki) and [InkPlayground ](https://github.com/psiberx/cp2077-playground)for more examples and details.
