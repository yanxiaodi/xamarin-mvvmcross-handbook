### Setting the menu navigation

When clicking the menu item, the app should show the correct DetailPage. Now let us set the `Command` for the menu item. Open the `MenuViwModel.cs` file in the MvxFormsMasterDetailDemo.Core project, and add a command, as shown below:

```c#
        #region ShowDetailPageAsyncCommand;
        private IMvxAsyncCommand<string> _showDetailPageAsyncCommand;
        public IMvxAsyncCommand<string> ShowDetailPageAsyncCommand
        {
            get
            {
                _showDetailPageAsyncCommand = _showDetailPageAsyncCommand ?? new MvxAsyncCommand<string>(ShowDetailPageAsync);
                return _showDetailPageAsyncCommand;
            }
        }
        private async Task ShowDetailPageAsync(string param)
        {
            // Implement your logic here.
        }
        #endregion
```

This is a instance of `IMvxAsyncCommand<T>`, which contains a parameter from the data-binding. The type of the parameter is `string`, because we know the object in the `MenuItemList` is `string`. If you have some other generic types for the `MenuItemList` , remember to change the generic type to your real item type.

Then we need to set the data-binding for the command. Open the `MenuPage.xaml` file in the MvxFormsMasterDetailDemo.UI project and check current `ItemTemplate`:

```xaml
<ListView ItemsSource="{Binding MenuItemList}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <TextCell Text="{Binding}"></TextCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Here we just use a simple `TextCell` to show the menu text. How to bind the command to the `ListView`?

Before we start the data-binding, I recommend you to read this article: [The Xamarin.Forms Command Interface](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/data-binding/commanding). In Xamarin.Forms, some controls support `Command` inherently, such as `Button`, `MenuItem`, `TextCell` and some classes that derive from them. And `SearchBar` supports `SearchCommand` property that actually is an `ICommand` type. The `RefreshCommand` property of `ListView` is also an instance of `ICommand` interface.

For those controls that do not support `ICommand` directly, Xamarin.Forms provides a `TapGestureRecognizer` to support `Command` binding. For more details, please read this article: [Adding a tap gesture recognizer](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/gestures/tap). Keep in mind that although `GestureRecognizer` supports more gestures, such as `pinch`, `pan`, and `swipe`, only `TapGestureRecognizer` supports `ICommand`. Another limit is that the view element must support `GestureRecognizers`.

Now let me show you how to use `TapGestureRecognizer`  to bind the `Command` to the menu item. First, set the `x:Name` property of the `MenuPage`:

```xaml
<views:MvxContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:views="clr-namespace:MvvmCross.Forms.Views;assembly=MvvmCross.Forms"
             xmlns:viewModels="clr-namespace:MvxFormsMasterDetailDemo.Core.ViewModels;assembly=MvxFormsMasterDetailDemo.Core"
             x:Class="MvxFormsMasterDetailDemo.UI.Pages.MenuPage"
             x:TypeArguments="viewModels:MenuViewModel"
             x:Name="MainContent">
```

We need the name to reference the current ViewModel of the page.

Update the `ItemTemplate` like this:

```xaml
<ListView.ItemTemplate>
    <DataTemplate>
        <ViewCell>
            <StackLayout Padding="10">
                <StackLayout.GestureRecognizers>
                    <TapGestureRecognizer 
                        Command="{Binding BindingContext.DataContext.ShowDetailPageAsyncCommand, Source={x:Reference MainContent}}"
                        CommandParameter="{Binding}">
                    </TapGestureRecognizer>
                </StackLayout.GestureRecognizers>
                <Label Text="{Binding}" VerticalOptions="Center"></Label>
            </StackLayout>
        </ViewCell>
    </DataTemplate>
</ListView.ItemTemplate>

```

We made some changes to the `ItemTemplate`:

First, use `ViewCell` to replace the default `TextCell`. `ViewCell` provides us with more flexibility to customize the UI. So we can define the `ItemTemplate` as you like. For instance, maybe we will add a icon for every menu item.

In the `ViewCell` element, use a `StackLayout` control as the container, which supports `GestureRecognizers`, so we can add the `TapGestureRecognizer` to the `StackLayout`. 

In the `TapGestureRecognizer` element, I define two important properties, one is `Command`, another is `CommandParameter`. As I said in my previous articles, you must be very clear about your current `DataContext` that you bind to your view. For our case, I must find the command named `ShowDetailPageAsyncCommand` in the `MenuViewModel`, so I use `Source={x:Reference MainContent}` to get the source object, which is the current Page named `MainContent`. Now we can get the ViewModel of the page by `BindingContext.DataContext`, then use `BindingContext.DataContext.ShowDetailPageAsyncCommand` as the binding path. I have been a little bit confused about `BindingContext.DataContext` because it is different from the syntax in UWP. Notice that the full syntax is: 

```xaml
Command="{Binding Path=BindingContext.DataContext.ShowDetailPageAsyncCommand, Source={x:Reference MainContent}}"
```

When you put the `Path` for the first parameter of the command-binding, it can be eliminated. 

For the `CommandParameter`, it is easier. Just bind the current string to it. So it is `CommandParameter="{Binding}"`. If you use an object that contains some properties, just use `CommandParameter="{Binding YourProperty}"`.

Next, let us update the command. Open `MenuViewModel.cs` files in the `ViewModels` folder in the MvxFormsMasterDetailApp.Core project again, and complete the `ShowDetailPageAsync` method, as the following code:

```c#
        private async Task ShowDetailPageAsync(string param)
        {
            // Implement your logic here.
            switch (param)
            {
                case "Contacts":
                    await mvxNavigationService.Navigate<ContactsViewModel>();
                    break;
                case "Todo":
                    await mvxNavigationService.Navigate<TodoViewModel>();
                    break;
                default:
                    break;
            }
        }
        #endregion
```

This method receives a parameter, which come from the command-binding. So we can determine which page should be displayed as the DetailPage.

Now launch the app and observe the navigation behavior. There is another problem. When clicking the menu item, although the DetailPage shows correctly, the MenuPage still covers the DetailPage. So we must control the navigation behavior of the MasterPage. To do it, we need to install Xamarin.Forms to the MvxFormsMasterDetailDemo.Core project. You can install it by searching `Xamarin.Forms` in the NuGet Package Manager. Please install the same version of Xamarin.Forms with the other project to avoid reference errors. For my demo solution, I use `Xamarin.Forms.3.4.0.1008975`.

Add some code into the `ShowDetailPageAsync` method after the `switch` segment:

```c#
if (Application.Current.MainPage is MasterDetailPage masterDetailPage)
{
    masterDetailPage.IsPresented = false;
}
else if (Application.Current.MainPage is NavigationPage navigationPage
         && navigationPage.CurrentPage is MasterDetailPage nestedMasterDetail)
{
    nestedMasterDetail.IsPresented = false;
}
```

`IsPresented` is used to control whether the master page is presented. To hide the MasterPage, set it to `false`. For more details, read it here: [Creating and Displaying the Detail Page](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/navigation/master-detail-page#creating-and-displaying-the-detail-page).

Launch the app for all three platforms to make sure the menu work properly.