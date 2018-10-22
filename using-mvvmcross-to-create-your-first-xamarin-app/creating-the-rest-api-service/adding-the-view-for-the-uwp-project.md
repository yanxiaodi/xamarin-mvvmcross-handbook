# Adding the View for the UWP project

Add a button to the `FirstView.xaml` in the `Views` folder of the `MvvmCrossDemo.Uwp` project and bind the `NavToPostListAsyncCommand` command of the `FirstViewModel.cs`, as shown below:

```markup
<TextBlock Text="{Binding Greeting}"></TextBlock>
        <Button Content="Post List" Command="{Binding NavToPostListAsyncCommand}"></Button>
```

Create a new Blank Page item called `PostListView.xaml` to the Views folder in the `MvvmCrossDemo.Uwp` project. Open `PostListView.xaml.cs` and change the base class to `MvxWindowsPage` class, like this:

```csharp
namespace MvvmCrossDemo.Uwp.Views
{
    [MvxViewFor(typeof(PostListViewModel))]
    public sealed partial class PostListView : MvxWindowsPage
    {
        public PostListView()
        {
            this.InitializeComponent();
        }
    }
}
```

Open `PostListView.xaml` and replace the content with the following codes:

```markup
<views:MvxWindowsPage
    x:Class="MvvmCrossDemo.Uwp.Views.PostListView"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
xmlns:views="using:MvvmCross.Platforms.Uap.Views" >
    <Grid>
    </Grid>
</local:BaseView>
```

Add a `ListView` control to the root layout and define the item template, like this:

```markup
    <Grid>
        <ListView ItemsSource="{Binding PostList}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel HorizontalAlignment="Stretch">
                        <TextBlock Text="{Binding Title}" Style="{StaticResource TitleTextBlockStyle}"></TextBlock>
                        <TextBlock Text="{Binding Body}"></TextBlock>
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
```

Open the `Package.appxmanifest` file and select Capabilities tab, and make sure the Internet \(Client\) option is checked. Here is the result:

![](../../.gitbook/assets/image%20%2822%29.png)

