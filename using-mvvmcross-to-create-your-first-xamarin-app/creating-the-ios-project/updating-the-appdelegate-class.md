# Updating the AppDelegate class

Open the `AppDelegate.cs` file and change the `AppDelegate` class to inherit from `MvxApplicationDelegate<MvxIosSetup<MvvmCrossDemo.Core.App>, MvvmCrossDemo.Core.App>` instead of `UIApplicationDelegate`. You can find the source code of it here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/iOS/AppDelegate.cs.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/iOS/AppDelegate.cs.pp).

Then delete all the methods in this class. The result looks like this:

```csharp
using Foundation;
using MvvmCross.Platforms.Ios.Core;
using UIKit;

namespace MvvmCrossDemo.iOS
{
    // The UIApplicationDelegate for the application. This class is responsible for launching the
    // User Interface of the application, as well as listening (and optionally responding) to application events from iOS.
    [Register("AppDelegate")]
    public class AppDelegate : MvxApplicationDelegate<MvxIosSetup<Core.App>, Core.App>
    {
    }
}
```

