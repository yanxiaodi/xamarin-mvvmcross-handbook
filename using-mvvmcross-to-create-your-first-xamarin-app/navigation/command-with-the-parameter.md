# Command with the parameter

Now come back to the `PostListViewModel`. Inject the instance of the `IMvxNavigationService` in the constructor, as shown below:

```csharp
private readonly IPostService _postService;
private readonly IMvxNavigationService _navigationService;
public PostListViewModel(IPostService postService, IMvxNavigationService navigationService)
{
    _postService = postService;
    _navigationService = navigationService;
}
```

Add a new command called `ShowPostDetailAsyncCommand` in the `PostListViewModel` to respond the item click event, as shown below:

```csharp
        #region ShowPostDetailAsyncCommand;
        private IMvxAsyncCommand<Post> _showPostDetailAsyncCommand;
        public IMvxAsyncCommand<Post> ShowPostDetailAsyncCommand
        {
            get
            {
                if (_showPostDetailAsyncCommand != null)
                {
                    return _showPostDetailAsyncCommand;
                }
                _showPostDetailAsyncCommand = new MvxAsyncCommand<Post>(async(post) => await ShowPostDetailAAsync(post));
                return _showPostDetailAsyncCommand;
            }
        }
        private async Task ShowPostDetailAsync(Post post)
        {
            // Implement your logic here.
            await _navigationService.Navigate<PostDetailViewModel, Post>(post);
        }
        #endregion
```

You can use the code snippet `mvxcmdap` to generate an `async` command. This command is an `async` command with a generic type `Post` as its param. When the user clicks the post item, I can get the current post id and pass it into the command.

For the `MvvmCrossDemo.Droid` project, update the data-binding in the `PostListView.axml` like this:

```markup
    <MvvmCross.Platforms.Android.Binding.Views.MvxListView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        local:MvxBind="ItemsSource PostList; ItemClick ShowPostDetailAsyncCommand"
        android:layout_weight="1"
        local:MvxItemTemplate="@layout/post_item" />

```

Now the `ListView` is able to respond the `ItemClick` event and pass the context \(the current Post\) to the `ShowPostDetailAsyncCommand`.

For the `MvvmCrossDemo.iOS` project, it is a little complicated. We need to create a new class called `PostListTableSource` that inherits from `MvxTableViewSource` to replace the default `MvxStandardTableViewSource`, then override the `RowSelected` method to response the item click event:

```csharp
public class PostListTableSource : MvxTableViewSource
{
    private static readonly NSString PostCellIdentifier = new NSString("PostCell");

    public PostListTableSource(UITableView tableView) : base(tableView)
    {
    }


    protected override UITableViewCell GetOrCreateCellFor(UITableView tableView, NSIndexPath indexPath, object item)
    {
        var cell = TableView.DequeueReusableCell(PostCellIdentifier, indexPath);
        cell.TextLabel.Text = ((WrapperPostViewModel)item).Post.Title;
        cell.DetailTextLabel.Text = ((WrapperPostViewModel)item).Post.Body;
        return cell;
    }

    public override void RowSelected(UITableView tableView, NSIndexPath indexPath)
    {
        base.RowSelected(tableView, indexPath);
        var item = this.SelectedItem;
        ((WrapperPostViewModel)item)?.ShowPostDetailAsyncCommand
            .Execute((WrapperPostViewModel)item);
    }
}
```

Then update the `PostListView` class like this:

```csharp
[MvxFromStoryboard(nameof(PostListView))]
public partial class PostListView : MvxTableViewController<PostListViewModel>
{
    private PostListTableSource _source;
    public PostListView (IntPtr handle) : base (handle)
    {
    }

    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
        _source = new PostListTableSource(TableView);
        TableView.Source = _source;

        var set = this.CreateBindingSet<PostListView, PostListViewModel>();
        set.Bind(_source).To(vm => vm.PostList);
        set.Apply();
        TableView.ReloadData();
    }
}
```

For the `MvvmCrossDemo.Uwp` project, we need to install Behaviors package first from the NuGet Package Manager. Or you can input the command below in the Package Manager Console:

```bash
Install-Package Microsoft.Xaml.Behaviors.Uwp.Managed
```

We use the behaviour to support event command binding. Update the codes of the `PostListView.xaml`, as shown below:

```markup
<views:MvxWindowsPage
    x:Class="MvvmCrossDemo.Uwp.Views.PostListView"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    xmlns:i="using:Microsoft.Xaml.Interactivity"
    xmlns:core="using:Microsoft.Xaml.Interactions.Core"
    xmlns:views="using:MvvmCross.Platforms.Uap.Views">
    <Grid>
        <ListView ItemsSource="{Binding PostList}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel HorizontalAlignment="Stretch">
                            <i:Interaction.Behaviors>
                                <core:EventTriggerBehavior EventName="Tapped">
                                    <core:InvokeCommandAction Command="{Binding ShowPostDetailAsyncCommand}"></core:InvokeCommandAction>
                                </core:EventTriggerBehavior>
                            </i:Interaction.Behaviors>
                            <TextBlock Text="{Binding Post.Title}" Style="{StaticResource TitleTextBlockStyle}"></TextBlock>
                            <TextBlock Text="{Binding Post.Body}"></TextBlock>
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</views:MvxWindowsPage>
```

You can also use some other `InvokeCommandAction` to bind the command of the `ViewModel`, for example, you can set the `IsItemClickEnabled` property of the `ListView` as `True` and bind the command to the `ItemClick` event. There is not only one approach to achieve the goal, so just select the one you like. Make sure you are clear about the `DataContext` of your current controls to avoid some unexpected binding errors.

