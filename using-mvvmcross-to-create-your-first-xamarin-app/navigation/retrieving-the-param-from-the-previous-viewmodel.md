# Retrieving the param from the previous ViewModel

In the `PostDetailViewModel`, we need to get the param from the `IMvxNavigationService`. Change the basic class of the `PostDetailViewModel` like this:

```csharp
public class PostDetailViewModel : MvxViewModel
```

Now it has a generic type constraint to receive the param. To do this, we need to update the Prepare override method, which is the right place to receive the param that is passed by the previous ViewModel. Add a private variable to store the post id:

```csharp
private int _postId;
```

Create a new object to store the comments:

```csharp
#region CommentList;
private MvxObservableCollection<Comment> _commentList;
public MvxObservableCollection<Comment> CommentList
{
    get => _commentList;
    set => SetProperty(ref _commentList, value);
}
#endregion
```

And update the `Prepare` method to initialize the variables like this:

```csharp
public override void Prepare(Post post)
{
    // This is the first method to be called after construction
    CommentList = new MvxObservableCollection<Comment>();
    _postId = post.Id;
}
```

After that, we can use the `_postId` variable in the Initialize method to load the data from the APIs, as shown below:

```csharp
public override async Task Initialize()
{
    // Async initialization, YEY!
    await base.Initialize();
    await GetPost(_postId);
    await GetComments(_postId);
}
```

Here is the result of the Android project:

![](../../.gitbook/assets/image%20%285%29.png)

For the UWP project:

![](../../.gitbook/assets/image%20%2837%29.png)

Let us get deep into the `IMvxNavigationService` in the next section.

