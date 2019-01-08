### Creating the XAML files

Add two ContentPage files into the `Pages` folder of MvxFormsMasterDetailDemo.UI project and name them as `ContactsPage.xaml` and `TodoPage.xaml`.  To use MvvmCross feature, we need to change them to inherit from `MvxContentPage`. Open the ContactsPage.xaml file and replace the content by the following code:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<views:MvxContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MvxFormsMasterDetailDemo.UI.Pages.ContactsPage"
             xmlns:views="clr-namespace:MvvmCross.Forms.Views;assembly=MvvmCross.Forms"
             xmlns:viewModels="clr-namespace:MvxFormsMasterDetailDemo.Core.ViewModels;assembly=MvxFormsMasterDetailDemo.Core"
             x:TypeArguments="viewModels:ContactsViewModel">
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Welcome to ContactsPage!"
                VerticalOptions="CenterAndExpand" 
                HorizontalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage.Content>
</views:MvxContentPage>
```

The Label is used to indicate the current page.

Open the `ContactsPage.xaml.cs` file and update the content like this:

```c#
using MvvmCross.Forms.Presenters.Attributes;
using MvvmCross.Forms.Views;
using MvxFormsMasterDetailDemo.Core.ViewModels;
using Xamarin.Forms.Xaml;

namespace MvxFormsMasterDetailDemo.UI.Pages
{
    [XamlCompilation(XamlCompilationOptions.Compile)]
	[MvxMasterDetailPagePresentation(Position = MasterDetailPosition.Detail, NoHistory = true, Title = "Contacts Page")]
    public partial class ContactsPage : MvxContentPage<ContactsViewModel>
	{
		public ContactsPage ()
		{
			InitializeComponent ();
		}
	}
}

```

The value of the `Position` property is `MasterDetailPosition.Detail`, which means this page should be located to the Detail area of the MasterDetailPage. The `NoHistory` property should be `true` so that there is no strange behavior for the navigation for different platforms. The `Title` property is used to show the page name on the top of the page.

Do the same changes to `TodoPage.xaml` and `TodoPage.xaml.cs` correspondingly. Do not forget to update the Text of the Label control to show the page name.