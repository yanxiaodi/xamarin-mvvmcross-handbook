# Creating the PostDetail View & ViewModel

I will not post all the codes here and you can get the codes from the GitHub repo: [https://github.com/yanxiaodi/MvvmCrossDemo](https://github.com/yanxiaodi/MvvmCrossDemo). I also created a new `Comment` model, a service interface and the implementation for comments. Now look at the codes in the `PostDetailViewModel`:

```csharp
namespace MvvmCrossDemo.Core.ViewModels
{
    public class PostDetailViewModel : MvxViewModel
    {
        private readonly IPostService _postService;

        public PostDetailViewModel(IPostService postService)
        {
            _postService = postService;
        }

        public override void Prepare()
        {
            // This is the first method to be called after construction
            CommentList = new MvxObservableCollection<Comment>();

        }

        public override async Task Initialize()
        {
            // Async initialization, YEY!

            await base.Initialize();
        }


        #region Post;
        private Post _post;
        public Post Post
        {
            get => _post;
            set => SetProperty(ref _post, value);
        }
        #endregion

        #region CommentList;
        private MvxObservableCollection<Comment> _commentList;
        public MvxObservableCollection<Comment> CommentList
        {
            get => _commentList;
            set => SetProperty(ref _commentList, value);
        }
        #endregion

        private async Task GetPost(int postId)
        {
            var response = await _postService.GetPost(postId);
            if (response.IsSuccess)
            {
                Post = response.Result;
            }
        }

        private async Task TaskGetComments(int postId)
        {
            var response = await _postService.GetComments(postId);
            if (response.IsSuccess)
            {
                CommentList.AddRange(response.Result);
            }
        }
    }
}
```

In this `ViewModel`, we have to get the `postId` from the previous view for the data initialization. But how to pass the param to the `PostDetailViewModel`?

