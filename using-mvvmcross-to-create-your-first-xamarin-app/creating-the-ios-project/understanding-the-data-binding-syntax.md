# Understanding the data-binding syntax

Override the `ViewDidLoad` method in the `FirstView.cs` by the following code:

```csharp
public override void ViewDidLoad()
{
      base.ViewDidLoad();
      var set = this.CreateBindingSet<FirstView, FirstViewModel>();
      set.Bind(txtUserName).To(vm => vm.UserName);
      set.Bind(lblGreeting).To(vm => vm.Greeting);
      set.Bind(btnShowGreeting).To(vm => vm.GetGreetingCommand);
      set.Apply();
}
```

`MvvmCross` use the fluent syntax to implement data-binding for iOS. First, `MvvmCross` creates the relationship between the View and the `ViewModel` by this code:

```csharp
var set = this.CreateBindingSet<FirstView, FirstViewModel>();
```

Then you can set the data-binding by `For` and `To` methods:

```csharp
set.Bind(txtUserName).For(x => x.Text).To(vm => vm.UserName);
```

If For is not provided, `MvvmCross` will set the default view property. For example, the `Text` property will be used for a `Label` control. For more default view properties information, check this document here: [https://www.mvvmcross.com/documentation/fundamentals/data-binding](https://www.mvvmcross.com/documentation/fundamentals/data-binding).

With the fluent syntax we can continue to specify the binding type, such as `OneWay`, and `TwoWay`, etc, like this:

```csharp
set.Bind(txtUserName).For(x => x.Text).To(vm => vm.UserName).TwoWay();
```

At last, **DO NOT** forget to call the `Apply` method to apply the data-binding:

```csharp
set.Apply();
```

We can build and launch the iOS project now. Here is the result:

![](../../.gitbook/assets/image%20%2816%29.png)

