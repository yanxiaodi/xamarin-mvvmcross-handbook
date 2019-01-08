### Adjust the height of the item for UWP

You might notice that if we use `TextCell` as the item template of the `ListView`, there are default margins and styles for the items in the `ListView` for Android and iOS. But for UWP platform, there is no default styles and proper height for items. Let us define the style for item template. At the same time, we should make sure it works for every platform.

Open the `MenuPage.xaml` file in `Pages` folder of the MvxFormsMasterDetailDemo.UI project. Update the `ItemTemplate` by the following code:

```xaml
<ListView.ItemTemplate>
    <DataTemplate>
        <ViewCell>
            <StackLayout HeightRequest="50">
                <Label Text="{Binding}" Margin="20,0,0,0" 
                       VerticalOptions="CenterAndExpand"></Label>
            </StackLayout>
        </ViewCell>
    </DataTemplate>
</ListView.ItemTemplate>
```

That is it. At last, the whole file looks like this:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<views:MvxContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:views="clr-namespace:MvvmCross.Forms.Views;assembly=MvvmCross.Forms"
             xmlns:viewModels="clr-namespace:MvxFormsMasterDetailDemo.Core.ViewModels;assembly=MvxFormsMasterDetailDemo.Core"
             x:Class="MvxFormsMasterDetailDemo.UI.Pages.MenuPage"
             x:TypeArguments="viewModels:MenuViewModel" 
             x:Name="MainContent"
             xmlns:behaviors="clr-namespace:Behaviors;assembly=Behaviors"
             Icon="hamburger.png">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout HeightRequest="40">
                <StackLayout.IsVisible>
                    <OnPlatform x:TypeArguments="x:Boolean">
                        <On Platform="Android, iOS" Value="True" />
                        <On Platform="UWP" Value="False" />
                    </OnPlatform>
                </StackLayout.IsVisible>
                <StackLayout.Margin>
                    <OnPlatform x:TypeArguments="Thickness">
                        <On Platform="iOS" Value="0,20,0,0" />
                    </OnPlatform>
                </StackLayout.Margin>
                <Label Text="HamburgerMenu Demo" Margin="10" VerticalOptions="Center" FontSize="Large"></Label>
            </StackLayout>
            <ListView x:Name="MenuList" ItemsSource="{Binding MenuItemList}" 
                      SelectedItem="{Binding SelectedMenuItem, Mode=TwoWay}">
                <ListView.Behaviors>
                    <behaviors:EventHandlerBehavior EventName="ItemSelected">
                        <behaviors:InvokeCommandAction 
                            Command="{Binding BindingContext.DataContext.ShowDetailPageAsyncCommand, 
                            Source={x:Reference MainContent}}"></behaviors:InvokeCommandAction>
                        </behaviors:EventHandlerBehavior>
                </ListView.Behaviors>
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <ViewCell>
                            <StackLayout HeightRequest="50">
                                <Label Text="{Binding}" Margin="20,0,0,0" 
                                       VerticalOptions="CenterAndExpand"></Label>
                            </StackLayout>
                        </ViewCell>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage.Content>
</views:MvxContentPage>
```

It is time to launch the app for all three platforms and observe the final results! Now the application shows correct Hamburger menu for three platforms and it has proper margin and styles.