# Updating the App.xaml.cs and the App.xaml

Open the `App.xaml.cs` file and replace the content with the following code:

```csharp
using MvvmCross.Platforms.Uap.Core;
using MvvmCross.Platforms.Uap.Views;

namespace MvvmCrossDemo.Uwp
{
    /// <summary>
    /// Provides application-specific behavior to supplement the default Application class.
    /// </summary>
    sealed partial class App
    {
        /// <summary>
        /// Initializes the singleton application object.  This is the first line of authored code
        /// executed, and as such is the logical equivalent of main() or WinMain().
        /// </summary>
        public App()
        {
            this.InitializeComponent();
        }
    }

    public abstract class UWPApplication : MvxApplication<MvxWindowsSetup<Core.App>, Core.App>
    {
    }
}
```

You can find the codes here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/UWP/App.xaml.cs.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/UWP/App.xaml.cs.pp) .

`UwpApplication` is an abstract class that contains the necessary information to run the `MvvmCross` framework. We can also use our customized `Setup` class instead of the default `Setup` class. Just keep it as simple as possible at the moment.

Now it is time to update the `App.xaml`. We need to modify the `XAML` code to make sure the `App` class inherits from the `UwpApplication`. You can find it here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/UWP/App.xaml.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/UWP/App.xaml.pp) . Replace the content of the file with the following code:

```markup
<local:UwpApplication
    x:Class="MvvmCrossDemo.Uwp.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MvvmCrossDemo.Uwp">
    <Application.Resources>
        <x:String x:Key="WelcomeText">Hello World!</x:String>
    </Application.Resources>
</local:UwpApplication>
```



