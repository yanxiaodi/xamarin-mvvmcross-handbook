# Responding the events from different controls in the ListView

I have created the `UpdatePost` method in the `IPostService`/`PostService`, and the Views in the two specific platform projects. You can copy them into your projects. But before we start this function, I would like to make a small change. Create a new folder named `Dto` in the `Models` folder in the `MvvmCrossDemo.Core` project and move the current three model files into the `Dto` folder. The `DTO` \(Data Transfer Object\) is an object that carries data between processes, that means these models are only used to transfer the `POCO` data from the APIs and the clients. Create a new file named `PostViewModel.cs` in the `Models` folder and replace the content with these codes:

```csharp
namespace MvvmCrossDemo.Core.Models
{
    public class PostViewModel: MvxViewModel
    {

        #region UserId;
        private int _userId;
        public int UserId
        {
            get => _userId;
            set => SetProperty(ref _userId, value);
        }
        #endregion


        #region Id;
        private int _id;
        public int Id
        {
            get => _id;
            set => SetProperty(ref _id, value);
        }
        #endregion


        #region Title;
        private string _title;
        public string Title
        {
            get => _title;
            set => SetProperty(ref _title, value);
        }
        #endregion


        #region Body;
        private string _body;
        public string Body
        {
            get => _body;
            set => SetProperty(ref _body, value);
        }
        #endregion
    }
}
```

You can use the code snippet `mvxpropdp` and press Tab to generate the codes quickly. Now the `PostViewModel` has the capability to support the property-binding, so we do not need to create every property of the current `Post` object as a variable in the `PostEditViewModel`. Once any property of the current `Post` object is updated by users, the UI will update automatically.

Then install the Auto-Mapper from Nuget Package Manger to simplify the mapping between the Post and the `PostViewModel`, or input the command below in Package Manager Console:

```bash
Install-Package AutoMapper
```

`AutoMapper` is a library to simplify the mapping between different models. It is optional and you can also convert them manually by setting every values for properties. Create a new file named `ModelMapping.cs` in the `Models` folder and update the content as shown below:

```csharp
namespace MvvmCrossDemo.Core.Models
{
    public static class ModelMapping
    {
        public static void Init()
        {
            AutoMapper.Mapper.Initialize(cfg =>
            {
                cfg.CreateMap<Post, PostViewModel>();
                cfg.CreateMap<PostViewModel, Post>();
            });
        }
    }
}

```

Update the `Initialize` method in the `App.cs` in the `MvvmCrossDemo.Core` project to call the `Init` method:

```csharp
        public override void Initialize()
        {
            CreatableTypes()
                .EndingWith("Service")
                .AsInterfaces()
                .RegisterAsLazySingleton();
            RegisterAppStart<FirstViewModel>();
            ModelMapper.Init();
        }
```

I am going to add a `Button` control for every `Post` item in the `PostListView` and add a command binding to the `Button`. I cannot use the `ItemClick` event because it will respond to the event for all the item UI area. To support multiple events for one list item, we need to respond to the events of different controls, such as the `TextView` and the `Button`, etc. As a result, we need to change the data context of the list item to make sure it contains associated commands to respond to the different events.

One of the solutions for this requirement is to create a wrapper class called `WrapperPostViewModel.cs` in the `Models` folder in the `MvvmCrossDemo.Core` project, as shown below:

```csharp
namespace MvvmCrossDemo.Core.Models
{
    public class WrapperPostViewModel: MvxViewModel
    {
        public PostViewModel Post { get; set; }

        public WrapperPostViewModel(PostViewModel post, Action<PostViewModel> showPostDetailAction, Action<PostViewModel> editPostAction)
        {
            Post = post;
            ShowPostDetailAsyncCommand = new MvxCommand(() => showPostDetailAction(this.Post));
            EditPostAsyncCommand = new MvxCommand(() => editPostAction(this.Post));
        }

        #region ShowPostDetailAsyncCommand;
        public IMvxCommand ShowPostDetailAsyncCommand { get; }

        #endregion

        #region EditPostAsyncCommand;
        public IMvxCommand EditPostAsyncCommand { get; }
        #endregion
    }
} 
```

In the `WrapperPostViewModel` class, there is a public property called `Post` to implement the data-binding as before. I added two commands to respond to the different events: one is for navigating to the `PostDetailView`, and another is for navigating to the `PostEditView`. These two commands will be initialized in the constructor method, which receives two `Action` methods as its parameters. When we create the instance of the `WrapperPostViewModel` in the `PostListViewModel`, we need to specify the Actions for these two commands.

Open the `PostListViewModel.cs`, update the type of the `PostList` property from `MvxObservableCollectio<Post>` to `MvxObservableCollection<WrapperPostViewModel>`. Delete the `ShowPostDetailAsyncCommand` and only keep the `ShowPostDetailAsync` method. In the `GetPostAsync` method we need to convert the result from Post to `WrapperPostViewModel`. Here are the updated codes:

```csharp
namespace MvvmCrossDemo.Core.ViewModels
{
    public class PostListViewModel : MvxViewModel
    {
        private readonly IPostService _postService;
        private readonly IMvxNavigationService _navigationService;

        public PostListViewModel(IPostService postService, IMvxNavigationService navigationService)
        {
            _postService = postService;
            _navigationService = navigationService;
        }

        public override void Prepare()
        {
            // This is the first method to be called after construction
            PostList = new MvxObservableCollection<WrapperPostViewModel>();
            
        }

        public override async Task Initialize()
        {
            // Async initialization, YEY!

            await base.Initialize();
            await GetPostsAsync();

        }
        
        #region PostList;
        private MvxObservableCollection<WrapperPostViewModel> _postList;
        public MvxObservableCollection<WrapperPostViewModel> PostList
        {
            get => _postList;
            set => SetProperty(ref _postList, value);
        }
        #endregion

        //#region GetPostsCommand;
        private async Task GetPostsAsync()
        {
            // Implement your logic here.
            var response = await _postService.GetPosts();
            if (response.IsSuccess)
            {
                PostList.AddRange(Mapper.Map<List<PostViewModel>>(response.Result)
                    .Select(x => new WrapperPostViewModel(x, ShowPostDetailAsync, EditPostAsync)));
            }
        }
        //#endregion

        
        private async void ShowPostDetailAsync(PostViewModel post)
        {
            await _navigationService.Navigate<PostDetailViewModel, PostViewModel>(post);
        }

        private async void EditPostAsync(PostViewModel post)
        {
            await _navigationService.Navigate<PostEditViewModel, PostViewModel>(post);
        }
    }
}
```

Now we have two methods to respond to the different click events. Open the `PostListView.axml` file in the `MvvmCrossDemo.Droid` project and delete the command-binding because we do not need `ItemClick` event. Open `post_item.axml` and replace the content with the following codes:

```csharp
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:local="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">
    <TextView
        android:id="@+id/postTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="@string/abc_font_family_title_material"
        android:clickable="true"
        local:MvxBind="Text Post.Title; Click ShowPostDetailAsyncCommand" />
    <TextView
        android:id="@+id/postBody"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/postTitle"
        android:clickable="true"
        local:MvxBind="Text Post.Body; Click ShowPostDetailAsyncCommand" />
    <Button
        android:id="@+id/buttonEdit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Edit"
        android:layout_alignParentRight="true"
        android:layout_below="@id/postBody"
        local:MvxBind="Click EditPostAsyncCommand" />
</RelativeLayout>
```

Be aware of the data context of the current item. Before we updated, the type of the data context is the `Post` class, now it is the `WrapperPostViewModel` class. So we can directly bind the commands to the different controls.

We also need to make some changes to `PostDetailViewModel`. Open the `PostDetailViewModel.cs` then change the `Post` class in the file to `PostViewModel` class. When getting the response from the API, convert the `Post` class to `PostViewModel` class by `AutoMapper`, as shown below:

```csharp
        private async Task GetPost(int postId)
        {
            var response = await _postService.GetPost(postId);
            if (response.IsSuccess)
            {
                Post = AutoMapper.Mapper.Map<PostViewModel>(response.Result);
            }
        }
```

For the `MvvmCrossDemo.Uwp` project, we need to update the associated files like the Android project. Update the `PostListView.xaml` like this:

```markup
<local:BaseView
    x:Class="MvvmCrossDemo.Uwp.Views.PostListView"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MvvmCrossDemo.Uwp.Views"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    xmlns:i="using:Microsoft.Xaml.Interactivity"
    xmlns:core="using:Microsoft.Xaml.Interactions.Core">
    <Grid>
        <ListView ItemsSource="{Binding PostList}">
            <ListView.ItemContainerStyle>
                <Style TargetType="ListViewItem">
                    <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
                </Style>
            </ListView.ItemContainerStyle>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel HorizontalAlignment="Stretch">
                        <StackPanel>
                            <i:Interaction.Behaviors>
                                <core:EventTriggerBehavior EventName="Tapped">
                                    <core:InvokeCommandAction Command="{Binding ShowPostDetailAsyncCommand}"></core:InvokeCommandAction>
                                </core:EventTriggerBehavior>
                            </i:Interaction.Behaviors>
                            <TextBlock Text="{Binding Post.Title}" Style="{StaticResource TitleTextBlockStyle}"></TextBlock>
                            <TextBlock Text="{Binding Post.Body}"></TextBlock>
                        </StackPanel>
                        <Button Content="Edit" RelativePanel.Below="TxtBody" HorizontalAlignment="Right"
                                Command="{Binding EditPostAsyncCommand}"></Button>
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</local:BaseView>

```

The root `StackPanel` control is divided into two areas to respond different commands. The current data context of the `ListViewItem` is the instance of the `WrapperPostViewModel` which contains two commands, so we can directly bind the command without specifying the data context. It is a good idea to create a new template file for the list item if you would like to do it.

