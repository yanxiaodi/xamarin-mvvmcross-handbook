# Creating the Android project

Add a new Xamarin Android project with name `MvvmCross.Forms.Droid` into the solution. **DO NOT** select the Android XAML App \(Xamarin.Forms\) template item.

![](../../.gitbook/assets/image%20%2828%29.png)

Select the Blank App template:

![](../../.gitbook/assets/image%20%2849%29.png)

Install the `Xamarin.Forms` package from the NuGet Package Manageer, or input the command below in the NuGet Manager Console:

```bash
Install-Package Xamarin.Forms
```

Then install the `MvvmCross.Forms` package from the NuGet Package Manager by searching MvvmCross.forms:

![](../../.gitbook/assets/image%20%2813%29.png)

If you prefer the Package Manager Console, please use this command:

```bash
Install-Package MvvmCross.Forms
```

It will take a while since it need to install some related packages. Next, add the references to both `MvvmCrossDemo.Core` project and `MvvmCrossDemo.Forms.UI` project:

![](../../.gitbook/assets/image%20%2817%29.png)

Also, we need to add the reference to `Mono.Android.Export` assembly:

![](../../.gitbook/assets/image%20%2851%29.png)

Delete the `activity_main.axml` file in the Resources\layout folder because we use the `MvvmCrossDemo.Forms.UI` project to host the UI.

Open the `MainActivity.cs` file and delete the automatically generated methods and make it to inherit from `MvxFormsAppCompatActivity<MvxFormsAndroidSetup<Core.App, UI.App>, Core.App, UI.App>` instead of `AppCompatActivity`, as shown below:

```csharp
using Android.App;
using MvvmCross.Forms.Platforms.Android.Core;
using MvvmCross.Forms.Platforms.Android.Views;

namespace MvvmCrossDemo.Forms.Droid
{
    [Activity(Label = "MvvmCrossDemo.Forms.Droid", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : MvxFormsAppCompatActivity<MvxFormsAndroidSetup<Core.App, UI.App>, Core.App, UI.App>
    {
    }
}
```

The base class of the `MainActivity` shows that the Android project uses the default Setup class to initialize itself. Notice that now we have two App classes, one is in the `MvvmCrossDemo.Core` project, another is in the `MvvmCrossDemo.Forms.UI` project.

Next, we need to define an `AppCompat` theme by adding a new XML file called `styles.xml` into the Resources/values folder. If it already exists, open it and replace the content with the following code:

```markup
<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>
  <style name="MainTheme" parent="MainTheme.Base">
  </style>
  <!-- Base theme applied no matter what API -->
  <style name="MainTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
    <!--If you are using revision 22.1 please use just windowNoTitle. Without android:-->
    <item name="windowNoTitle">true</item>
    <!--We will be using the toolbar so no need to show ActionBar-->
    <item name="windowActionBar">false</item>
    <!-- Set theme colors from http://www.google.com/design/spec/style/color.html#color-color-palette -->
    <!-- colorPrimary is used for the default action bar background -->
    <item name="colorPrimary">#2196F3</item>
    <!-- colorPrimaryDark is used for the status bar -->
    <item name="colorPrimaryDark">#1976D2</item>
    <!-- colorAccent is used as the default value for colorControlActivated
         which is used to tint widgets -->
    <item name="colorAccent">#FF4081</item>
    <!-- You can also set colorControlNormal, colorControlActivated
         colorControlHighlight and colorSwitchThumbNormal. -->
    <item name="windowActionModeOverlay">true</item>

    <item name="android:datePickerDialogTheme">@style/AppCompatDialogStyle</item>
  </style>

  <style name="AppCompatDialogStyle" parent="Theme.AppCompat.Light.Dialog">
    <item name="colorAccent">#FF4081</item>
  </style>
</resources>
```

You can find the code of the `style.xml` here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/AndroidContent/styles.xml.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/AndroidContent/styles.xml.pp). Because in Android, we use `Toolbar` instead of the `ActionBar` so we must set this item:

```bash
<item name="windowActionBar">false</item>
```

In this file, you can also customize some values for the style of the Android project. Apply this style file by updating the attributes of the `MainActivity` class in the `MainActivity.cs` file, as shown below:

```csharp
[Activity(Label = "MvvmCrossDemo.Forms.Droid", Theme = "@style/MainTheme", MainLauncher = true)]
public class MainActivity : MvxFormsAppCompatActivity<MvxFormsAndroidSetup<Core.App, UI.App>, Core.App, UI.App>
{
}
```

Now you can launch the `MvvmCrossDemo.Forms.Droid` Project. The result is:

![](../../.gitbook/assets/image%20%2816%29.png)

