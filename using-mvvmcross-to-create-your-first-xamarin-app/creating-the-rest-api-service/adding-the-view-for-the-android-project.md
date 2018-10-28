# Adding the View for the Android project

Add a button to the `FirstView.axml` in the Resources\layout folder of the `MvvmCrossDemo.Droid` project, and set the command binding like this:

```markup
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        local:MvxBind="Text Greeting" />
    <Button android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Post List"
        local:MvxBind="Click NavToPostListAsyncCommand">
</LinearLayout>
```

It is time to bind the data to the `PostListView`. Add a new AXML file called `PostListView.axml` into the Resources/layout folder in the `MvvmCrossDemo.Droid` project. Replace the content with the following codes:

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:local="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
  <MvvmCross.Platforms.Android.Binding.Views.MvxListView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    local:MvxBind="ItemsSource PostList"
    android:layout_weight="1">
  </MvvmCross.Platforms.Android.Binding.Views.MvxListView>
</LinearLayout>
```

We use `MvxListView` which comes from `MvvmCross` to show the data. Find more built in binding controls here: [https://www.mvvmcross.com/documentation/fundamentals/data-binding](https://www.mvvmcross.com/documentation/fundamentals/data-binding).

Then Add an `AXML` file called `post_item.axml` to show the post item, like this:

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:local="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
  <TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    local:MvxBind="Text Title" />
</LinearLayout>
```

Now set the item template of the `MvxListView`, as shown below:

```markup
<MvvmCross.Platforms.Android.Binding.Views.MvxListView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    local:MvxBind="ItemsSource PostList"
    android:layout_weight="1"
    local:MvxItemTemplate="@layout/post_item">
 </MvvmCross.Platforms.Android.Binding.Views.MvxListView>
```

You can only name the item template file to lowercase according to the convention of `MvvmCross`, otherwise `MvvxCross` cannot find the correct template layout file.

Add a new class called `PostListView.cs` into the `Views` folder, which is used to build the connection between the `ViewModel` and the `View`. Replace the content with the following codes:

```csharp
using Android.App;
using Android.OS;
using MvvmCross.Platforms.Android.Presenters.Attributes;

namespace MvvmCrossDemo.Droid.Views
{
    [MvxActivityPresentation]
    [Activity(Label = "MvvmCross Demo", MainLauncher = true)]
    public class PostListView : BaseView
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            SetContentView(Resource.Layout.PostListView);
        }
    }
}
```

Do not forget to enable the permission to connect to the Internet. Right-click the `MvvmCrossDemo.Droid` project and select properties, then select Android Manifest tab and update the Required permissions. Make sure the `INTERNET` option is checked.

Run the App and click the `PostList` button to navigate to the `PostListView`. Now the List should show the correct data from the API, as shown below:

![](../../.gitbook/assets/image%20%2825%29.png)

Be careful, we should not call any heavy operations in the first View because it is not good for the user experience.

By the way, I encountered a strange error here. VS cannot deploy the App to my device \(it is a HUAWEI Mate 10\) with an error:

> \[INSTALL\_FAILED\_ALREADY\_EXISTS: Attempt to re-install MvvmCrossDemo.Droid without first uninstalling.

I tried to uninstall the App in the Settings - Applications but it still showed the error. At last, I remembered that there is a second system in my phone, which is a special feature of HUAWEI Mate 10. I entered the second system environment and found that if I uninstalled the App in the default system environment, it still existed in the second system environment. So if you have a phone with the special multiple system feature, make sure to completely uninstall the App in all of these environments.

