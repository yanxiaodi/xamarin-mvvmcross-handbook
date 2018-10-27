# Adding the UWP View

Create a new folder called `Views` into the root folder of the `MvvmCrossDemo.Uwp` project. Add a Blank Page file called `FirstView.xaml` into the `Views` folder.

![](../../.gitbook/assets/image%20%2833%29.png)

Open the `FirstView.xaml.cs` file and Change the base class of the `FirstView` class to `MvxWindowsPage`, like this:

```csharp
using MvvmCross.Platforms.Uap.Views;
using MvvmCross.ViewModels;
using MvvmCrossDemo.Core.ViewModels;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=234238

namespace MvvmCrossDemo.Uwp.Views
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    [MvxViewFor(typeof(FirstViewModel))]
    public sealed partial class FirstView : MvxWindowsPage
    {
        public FirstView()
        {
            this.InitializeComponent();
        }
    }
}
```

You might notice that we added a `MvcViewForAttribute` to the `FirstView` class. So `MvvmCross` can attach the right `ViewModel` to it.

Open the `FirstView.xaml` file, and replace the content with these codes:

```markup
<views:MvxWindowsPage
    x:Class="MvvmCrossDemo.Uwp.Views.FirstView"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MvvmCrossDemo.Uwp.Views"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:views="using:MvvmCross.Platforms.Uap.Views"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <StackPanel>
        <TextBlock Text="Your Name"></TextBlock>
        <TextBox Text="{Binding UserName, Mode=TwoWay}"></TextBox>
        <Button Content="Click Me!" Command="{Binding GetGreetingCommand}"></Button>
        <TextBlock Text="{Binding Greeting}"></TextBlock>
    </StackPanel>
</views:MvxWindowsPage>
```

Make sure to change the Page to `views:MvxWindowsPage` and add the namespace reference by this code:

```csharp
xmlns:views="using:MvvmCross.Platforms.Uap.Views"
```

