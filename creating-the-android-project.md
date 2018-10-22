# Creating the Android project

Create a new Blank Android project into the solution \(do not select the Android XAML App template\) and name it as `MvvmCrossDemo.Droid`, like this:

![](.gitbook/assets/image%20%2840%29.png)

Pay attention on the different project templates. There are a couple of different templates here. Notice that we need the Android App \(Xamarin\), not the Android XAML App \(Xamarin.Forms\). I will describe `Xamarin.Forms` in the next sections, but not now.

The default Android target version is Android 8.1 \(Oreo\). If you need to edit it, right click the `MvvmCrossDemo.Droid` project and select Properties, then choose Application Tab, and update the Target Framework like this:

![](.gitbook/assets/image%20%288%29.png)

Delete the `MainActivity.cs` in the root folder and the `Main.axml`in the resource/Layout folder. We do not need them.

Then install the `MvvmCross` package like we did in the `MvvmCrossDemo.Core` project. We also need to add the reference to the `MvvmCrossDemo.Core` project. You might encounter an error that is: 

> You need to add a reference to Mono.Android.Export.Dll when you use ExportAttribute or ExportFieldAttribute.

To fix it, we need to add the reference to `Mono.Android.Export` by Reference Manager:

![](.gitbook/assets/image%20%2832%29.png)

