# Adding an Android Application class

Create a new activity class called `SpashScreen` to the root folder of the `MvvmCrossDemo.Droid` project. Then update it like this:

```csharp
using Android.App;
using Android.Content.PM;
using MvvmCross.Core;
using MvvmCross.Platforms.Android.Core;
using MvvmCross.Platforms.Android.Views;

namespace MvvmCrossDemo.Droid
{
    [Activity(
        Label = "MvvmCrossDemo.Droid"
        , MainLauncher = true
        , NoHistory = true
        , ScreenOrientation = ScreenOrientation.Portrait)]
    public class SplashScreen : MvxSplashScreenActivity<MvxAndroidSetup<Core.App>, Core.App>
    {
        public SplashScreen()
            : base(Resource.Layout.SplashScreen)
        {
            
        }
    }
} 
```

The `SplashScreen` class inherits from `MvxSplashScreenActivity, Core.App>`. You can find the source code here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Android/SplashScreen.cs.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Android/SplashScreen.cs.pp).

The value of the `NoHistory` property in the attribute is true so if users press the back button the splash screen should not be returned.

This is the basic setting for the Android application to make sure it follows the pattern of `MvvmCross`. For simplicity, we use the `MvxAndroidSetup` class, which is the default `Setup` class and takes responsibility for bootstrapping `MvvmCross` and registering platform service. We can use a customized `Setup` class and get more details in the next sections. Note that if you changed the name of `App` class in the Core project, you need to update it here.

