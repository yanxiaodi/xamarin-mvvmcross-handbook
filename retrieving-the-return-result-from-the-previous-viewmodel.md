# Retrieving the return result from the previous ViewModel

Now it is time to deal with the Edit function. You can find the `PostEditView` for specific-platform projects on my GitHub: [https://github.com/yanxiaodi/MvvmCrossDemo](https://github.com/yanxiaodi/MvvmCrossDemo).

Create a new class file named `PostEditViewModel.cs` in the `ViewModels` folder in the `MvvmCrossDemo.Core` project, as shown below:

```csharp
namespace MvvmCrossDemo.Core.ViewModels
{
    public class PostEditViewModel : MvxViewModel<PostViewModel>
    {
        private readonly IPostService _postService;
        private readonly IMvxNavigationService _navigationService;

        private int _postId;


        public PostEditViewModel(IPostService postService, IMvxNavigationService navigationService)
        {
            _postService = postService;
            _navigationService = navigationService;
        }

        public override void Prepare(PostViewModel post)
        {
            // This is the first method to be called after construction
            _postId = post.Id;
        }

        public override async Task Initialize()
        {
            // Async initialization, YEY!

            await base.Initialize();
            await GetPost(_postId);
        }


        #region Post;
        private PostViewModel _post;
        public PostViewModel Post
        {
            get => _post;
            set => SetProperty(ref _post, value);
        }
        #endregion


        private async Task GetPost(int postId)
        {
            var response = await _postService.GetPost(postId);
            if (response.IsSuccess)
            {
                Post = AutoMapper.Mapper.Map<PostViewModel>(response.Result);
            }
        }
    }
}
```

There are a lot of similarities between the `PostDetailViewModel` and the `PostEditViewModel`. We need to receive the current post id and get the data from the APIs. But for `PostEditViewModel`, we need to edit the `Post` and pass the result to the `PostListViewModel` to update the UI.

Come back to check the `IMvxNavigationService` interface. There is an interface that can return a result to the previous `ViewModel`, as shown below:

```csharp
Task<bool> Close<TResult>(IMvxViewModelResult<TResult> viewModel, TResult result);
```

And another interface to receive the result:

```csharp
Task<TResult> Navigate<TViewModel, TParameter, TResult>(TParameter param, IMvxBundle presentationBundle = null, CancellationToken cancellationToken = default(CancellationToken)) where TViewModel : IMvxViewModel<TParameter, TResult>;
```

Open the `PostEditViewModel.cs` and change the parent class of it from `MvxViewModel<PostViewModel>` to `MvxViewModel<PostViewModel, Post>`, that means it will return a result which is the `Post` type. Add two new commands here:

```csharp
        #region CancelAsyncCommand;
        private IMvxAsyncCommand _cancelAsyncCommand;
        public IMvxAsyncCommand CancelAsyncCommand
        {
            get
            {
                _cancelAsyncCommand = _cancelAsyncCommand ?? new MvxAsyncCommand(CancelAsync);
                return _cancelAsyncCommand;
            }
        }
        private async Task CancelAsync()
        {
            // Implement your logic here.
            await _navigationService.Close(this);
        }
        #endregion


        #region EditPostAsyncCommand;
        private IMvxAsyncCommand _editPostAsyncCommand;
        public IMvxAsyncCommand EditPostAsyncCommand
        {
            get
            {
                _editPostAsyncCommand = _editPostAsyncCommand ?? new MvxAsyncCommand(EditPostAsync);
                return _editPostAsyncCommand;
            }
        }
        private async Task EditPostAsync()
        {
            // Implement your logic here.
            var response = await _postService.UpdatePost(Post.Id, AutoMapper.Mapper.Map<Post>(Post));
            if (response.IsSuccess)
            {
                await _navigationService.Close(this, response.Result);
            }
        }
        #endregion

```

Notice that the resource will not be really updated on the server but it will be faked as if. For more details about the fake API here: [https://jsonplaceholder.typicode.com](https://jsonplaceholder.typicode.com).

There are two buttons to respond to the user’s action. Each of them will call the `Close` method but for the `Cancel` button, it will return a null as the result. When the user clicks the `Ok` button, the `EditPostAsync` method will post the update to the API and if the result is a success, another `Close` method will be called so the `MvxNavigationService` will close the current `ViewModel` and return a result to the previous `ViewModel`.

Then update the `EditPostAsync` method in the `PostListViewModel.cs` file to receive the result, as shown below:

```csharp
        private async void EditPostAsync(PostViewModel post)
        {
            var result = await _navigationService.Navigate<PostEditViewModel, PostViewModel, Post>(post);
            if (result != null)
            {
                var target = PostList.FirstOrDefault(x => x.Post.Id == result.Id);
                if (target != null)
                {
                    target.Post.Title = result.Title;
                    target.Post.Body = result.Body;
                }
            }
        }
```

Launch the App for the Android and the UWP. Now you can edit the Post. When you return to the `PostListView`, you should notice that the Post has been updated.

We did a lot of changes in this section. Be aware of the data context of your current controls to implement the data-binding. If you found that the command does not work well, check the data context first to make sure the command is correctly binding to the control.

