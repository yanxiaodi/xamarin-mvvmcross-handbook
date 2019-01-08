### Creating the XAML file

Next, add a new `ContentPage` called `MenuPage.xaml` into the `Pages` folder in the MvxFormsMasterDetailDemo.UI project. Open the `MenuPage.xaml` file and replace the content by the following code:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<views:MvxContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:views="clr-namespace:MvvmCross.Forms.Views;assembly=MvvmCross.Forms"
             xmlns:viewModels="clr-namespace:MvxFormsMasterDetailDemo.Core.ViewModels;assembly=MvxFormsMasterDetailDemo.Core"
             x:Class="MvxFormsMasterDetailDemo.UI.Pages.MenuPage"
             x:TypeArguments="viewModels:MenuViewModel" >
    <ContentPage.Content>
        <StackLayout>
            <ListView></ListView>
        </StackLayout>
    </ContentPage.Content>
</views:MvxContentPage>
```

Open the `MenuPage.xaml.cs` file and set the base class and the attributes, as shown below:

```c#
using MvvmCross.Forms.Presenters.Attributes;
using MvvmCross.Forms.Views;
using MvxFormsMasterDetailDemo.Core.ViewModels;
using Xamarin.Forms.Xaml;

namespace MvxFormsMasterDetailDemo.UI.Pages
{
    [XamlCompilation(XamlCompilationOptions.Compile)]
	[MvxMasterDetailPagePresentation(Position = MasterDetailPosition.Master, WrapInNavigationPage = false, Title = "HamburgerMenu Demo")]
    public partial class MenuPage : MvxContentPage<MenuViewModel>
	{
		public MenuPage ()
		{
			InitializeComponent ();
		}
	}
}

```

The `Position` property of the `MvxMasterDetailPagePresentation` attribute should be set to `Master`, which means that this page will be shown as the MasterPage of the MasterDetailPage. And there is another pitfall for the MasterPage: it must have the `Title` property set, otherwise, your application will be stuck. So you must set the `Title` property of the `MvxMasterDetailPagePresentation` attribute.

Now we need to set the data-binding for the `ListView`. We already have a `MenuItemList` in its `ViewModel`, so what we should do now is setting the `ItemsSource` of the `ListView`, like this:

```xaml
<ListView ItemsSource="{Binding MenuItemList}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <TextCell Text="{Binding}"></TextCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>

```

Currently, we just use the `TextCell` to show the menu text. Before we implement the full functionality of the hamburger menu, let us create the DetailPages.