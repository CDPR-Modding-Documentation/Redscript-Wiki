# Class constructors

Redscript does not have natively support constructors and to set the data of class fields you would have to either set them manually

```swift
public class CustomClass{
    //all the variables 
    public let myInt: Int32;
    public let myString: String;
}
```

```swift
let myCustomClass = new CustomClass();
myCustomClass.myInt = 1;
myCustomClass.myString = "Hello";
```

&#x20;or if you want to do it with a single clean line, you can define a static helper method

```swift
public class CustomClass{
    //all the variables 
    public let myInt: Int32;
    public let myString: String;
    
    public static func Create(inputInt: Int32, inputString: String) -> ref<CustomClass>{
        //you create the new instance of your class here instead of in your code
        //and set its variables
        let self = new CustomClass();
        self.myInt = inputInt;
        self.myString = inputString;
        return self;
    }
}
```

and how to use it

```swift
let myCustomClass = CustomClass.Create(1,"Hello");
```
