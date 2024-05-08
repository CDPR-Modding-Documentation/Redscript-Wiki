# DelaySystem and DelayCallback

DelaySystem is a class that allows async call of DelayCallback after the set amount of time has passed.

this is how to create a DelayCallback

```swift
public class CustomCallback extends DelayCallback{
    //all the data that your Call function needs
    private let myInt: Int32;
    
    public func Call() {
        //custom function that i want to be called when the specified time has passed
        if this.myInt>1 
        {
            LogChannel(n"DEBUG", "Input is bigger than 1");
        }
        else
        {
            LogChannel(n"DEBUG", "Input is smaller than 1");
        }
    }
    
    public static func Create(inputInt: Int32) -> ref<CustomCallback> {
        //use this way to create your Callback class in one line
        let self = new CustomCallback();
        self.myInt = inputInt;
        return self;
    }
}
```

and this is how you can use the DelaySystem to call the Callback function with a delay

```swift
let delaySystem = GameInstance.GetDelaySystem(this.m_player.GetGame());
let inputInt: Int32 = 2;
let delay: Float = 1.0;
let isAffectedByTimeDilation: Bool = false;
delaySystem.DelayCallback(CustomCallback.Create(inputInt), delay, isAffectedByTimeDilation);
```
