# Adding the View class

As I mentioned, we have added the `ViewModel` and the View, but we did not link them together. Now create a new folder called Views in the root folder of the `MvvmCrossDemo.Droid` project and add a class called `FirstView.cs` in the Views folder, as shown below:

```csharp
using Android.App;
using Android.OS;
using MvvmCross.Platforms.Android.Presenters.Attributes;
using MvvmCross.Platforms.Android.Views;

namespace MvvmCrossDemo.Droid.Views
{
    [MvxActivityPresentation]
    [Activity(Label = "First View")]
    public class FirstView : MvxActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            SetContentView(Resource.Layout.FirstView);
        }
    }
}
```

Make sure the `FirstView` inherits from the `MvxActivity` class. Now `MvvmCross` will help us build the connection between the View and the `ViewModel`.

Test the Android project on the phone. We should see the application like this:

![](../../.gitbook/assets/image%20%2831%29.png)

