# Creating the UWP project

Now let us create the UWP project based on `Xamarin.Forms`. Add a new Blank App \(Windows Universal\) project to the solution:

![](../../.gitbook/assets/image%20%2842%29.png)

Select the proper version of Windows 10:

![](../../.gitbook/assets/image%20%2821%29.png)

Install the `Xamarin.Forms` package in the NuGet Package Manager:

![](../../.gitbook/assets/image%20%2819%29.png)

Or use the command below in the Package Manager Console:

```bash
Install-Package MvvmCross.Forms
```

Then install the `MvvmCross.Forms` package in the NuGet Package Manager:

![](../../.gitbook/assets/image%20%2851%29.png)

Or you can input the command below in the Package Manager Console:

```bash
Install-Package MvvmCross.Forms
```

Then add the references to the `MvvmCrossDemo.Core` project and the `MvvmCrossDemo.Forms.UI` project by right-clicking the `MvvmCrossDemo.Forms.Uwp` project in the solution explorer:

![](../../.gitbook/assets/image%20%2833%29.png)

Open the `App.xaml.cs` file and add a new class with name `UWPApplication`, as shown below:

```csharp
public abstract class UWPApplication : MvxWindowsApplication<MvxFormsWindowsSetup<Core.App, UI.App>, Core.App, UI.App, MainPage>
{
}
```

Update the `App` class to inherit from `UWPApplication` class and remove all the other pre-generated methods except for the constructor. Only keep the `InitializeComponent()` method in the constructor. You can find the source code of this file here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/UWPContent/App.xaml.cs.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/UWPContent/App.xaml.cs.pp). At last, the `App.xaml.cs` look like this:

```csharp
using MvvmCross.Forms.Platforms.Uap.Core;
using MvvmCross.Forms.Platforms.Uap.Views;

namespace MvvmCrossDemo.Forms.Uwp
{
    /// <summary>
    /// Provides application-specific behavior to supplement the default Application class.
    /// </summary>
    sealed partial class App : UWPApplication
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

    public abstract class UWPApplication : MvxWindowsApplication<MvxFormsWindowsSetup<Core.App, UI.App>, Core.App, UI.App, MainPage>
    {
    }
}
```

Accordingly, we need update the `App.xaml` file to update the base class to make it to inherit from the `UWPApplication` class, as shown below:

```markup
<local:UWPApplication
    x:Class="MvvmCrossDemo.Forms.Uwp.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MvvmCrossDemo.Forms.Uwp">
</local:UWPApplication>
```

You can find the code of the App.xaml here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/UWPContent/App.xaml.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/UWPContent/App.xaml.pp).

Open the `MainPage.xaml` file and add the reference by this code:

```csharp
xmlns:forms="using:MvvmCross.Forms.Platforms.Uap.Views"
```

Change the class of the `MainPage` to `MvxFormsWindowsPage`. The result is:

```csharp
<forms:MvxFormsWindowsPage
    x:Class="MvvmCrossDemo.Forms.Uwp.MainPage"
    xmlns:forms="using:MvvmCross.Forms.Platforms.Uap.Views"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MvvmCrossDemo.Forms.Uwp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>

    </Grid>
</forms:MvxFormsWindowsPage>
```

You can find the source code of this file here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/UWPContent/MainPage.xaml.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/UWPContent/MainPage.xaml.pp).

Open the `MainPage.xaml.cs` and remove the base class of it, like this:

```csharp
namespace MvvmCrossDemo.Forms.Uwp
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
        }
    }
}
```

Find the sample code here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/UWPContent/MainPage.xaml.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/UWPContent/MainPage.xaml.pp).

Now the UWP project based on `Xamarin.Forms` is done. Launch it and you will see the result:

![](../../.gitbook/assets/image%20%2839%29.png)

