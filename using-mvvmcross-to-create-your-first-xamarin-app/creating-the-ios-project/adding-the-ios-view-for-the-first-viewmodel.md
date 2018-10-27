# Adding the iOS View for the first ViewModel

Keep in mind that although you can place multiple view controllers on one storyboard, the better way to use `MvvmCross` for iOS is to create one storyboard per view, which unify the navigation pattern with the other platforms. So we can have a clear structure, rather than placing multiple views in one storyboard.

To build and debug the iOS App, you need to pair to Mac first. When you build the project, you might encounter an error that is:

> Could not find any available provisioning profiles for iOS.

Open options dialog of VS 2017 and select Xamarin &gt; Apple Accounts, then click the `Install Fastlane` button. Also, make sure to check the Remote Simulator to Windows checkbox so we can use the iOS simulator on our Windows.

![](../../.gitbook/assets/image%20%2817%29.png)

Create a new folder called `Views` into the root folder of the `MvvmCrossDemo.iOS` project. Then create an empty `StoryBoard` called `FirstView.storyboard` to the `Views` folder:

![](../../.gitbook/assets/image%20%285%29.png)

Use the designer and drag a `ViewController` from the Toolbox to the  `FirstView.Storyboard` like this:

![](../../.gitbook/assets/image%20%288%29.png)

Click the bottom bar of the `ViewController` to select it:

![](../../.gitbook/assets/image%20%2855%29.png)

Then set the Class property and the Storyboard ID of it to `FirstView`. Make sure the Use Storyboard ID checkbox is checked, as shown below:

![](../../.gitbook/assets/image%20%2827%29.png)

Then it will create two files: the `FirstView.cs` and the `FirstView.designer.cs`. Move them to the `Views` folder. Change the namespace of the `FirstView.cs`  and the `FirstView.designer.cs` from `Blank` to `MvvmCrossDemo.iOS.Views`. Update the `FirstView` class to inherit from `MvxViewController<FirstViewModel>`, and add an attribute called `MvxFromStoryboard` with a parameter `FirstView` , which indicates the name of the storyboard file, as shown below:

```csharp
using MvvmCross.Binding.BindingContext;
using MvvmCross.Platforms.Ios.Views;
using MvvmCrossDemo.Core.ViewModels;
using System;

namespace MvvmCrossDemo.iOS.Views
{
    [MvxFromStoryboard("FistView")]
    public partial class FirstView : MvxViewController<FirstViewModel>
    {
        public FirstView (IntPtr handle) : base (handle)
        {
        }

    }
}
```

The Xamarin Designer for iOS allows us to drag and drop controls to edit the UI. Also, we can use it to set up the event handlers. Drag some controls from the Toolbox and place them on the `ViewController`:

* A `Lable` control. Set the `Text` property to `Your Name`; 
* A `Text Field` control to accept the input from users. Set the `Name` property to `txtUserName`; 
* A `Button` to respond the click event. Set the `Name` property to `btnShowGreeting` and the `Title` property to `Click Me!` .
* A `Label` to show the greeting message. Set the `Name` property to `lblGreeting`.

The controls must have the names so it can be accessed in the code. Now we have an UI like this:

![](../../.gitbook/assets/image%20%2841%29.png)

