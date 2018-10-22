# Creating the ViewModel

Add a new `ViewModel` file into the `ViewModel` folder and name it as `PostListViewModel.cs`. It should inherit from the `MvxViewModel` class. Inject the implementation for the `IPostService` in the constructor method, like this:

```csharp
private readonly IPostService _postService;

public PostListViewModel(IPostService postService)
{
     _postService = postService;
}
```

The constructor method is the right way to use the IoC container to inject all its dependencies.

Then add a new property for Data-Binding. You can type `mvxpropdp` and press `Tab` to complete the input, as shown as below:

```csharp
#region PostList;
private MvxObservableCollection<Post> _postList;
public MvxObservableCollection<Post> PostList
{
    get => _postList;
    set => SetProperty(ref _postList, value);
}
#endregion
```

Now we need to find a place to get the data when the page is loading. Find more details about the lifecycle of the `ViewModel` here: [https://www.mvvmcross.com/documentation/fundamentals/viewmodel-lifecycle](https://www.mvvmcross.com/documentation/fundamentals/viewmodel-lifecycle).

Let us focus on the `Initialize` method in the `ViewModel`, which is used to do some heavy loading operations, such as loading data from the APIs. It returns a `Task`, which can be marked as `async`. Add an override to this method, like this:

```csharp
public override async Task Initialize()
{
    // Async initialization, YEY!
    await base.Initialize();
    var response = await _postService.GetPostList();
    if (response.IsSuccess)
    {
        PostList = new MvxObservableCollection<Post>(response.Result);
    }
    else
    {
        // TODO； Show some error messages.
    }
}

```

That is why I use a `ResponseMessage` class to encapsulate the result from the APIs. If there are any problems about the `HTTP` request, this encapsulation is able to make sure the UI layer can deal with the response result correctly because all exceptions and the unexpected values have been handled in the Service layer. If you got a result, which has a `False` value to the `IsSuccess` property, show a friendly message to users, rather than throwing an exception.

Then we need to navigate to the `PostListViewModel` from the `FirstViewModel`. Update the constructor of the `FirstViewModel` to inject the `IMvxNavigationService`, as shown below:

```csharp
private readonly IGreetingService _greetingService;
private readonly IMvxNavigationService _navigationService;

public FirstViewModel(IGreetingService greetingService, IMvxNavigationService navigationService)
{
    _greetingService = greetingService;
    _navigationService = navigationService;
}
```

`IMvxNavigationService` is used to implement the navigation between views. I will describe more about it in the next section. Use the code snippet `mvxcmda` to create an `async` command in the `FirstViewModel.cs` to navigate to the `PostListViewModel`, like this:

```csharp
        #region NavToPostListAsyncCommand;
        private IMvxAsyncCommand _navToPostListAsyncCommand;
        public IMvxAsyncCommand NavToPostListAsyncCommand
        {
            get
            {
                _navToPostListAsyncCommand = _navToPostListAsyncCommand ?? new MvxAsyncCommand(NavToPostListAsync);
                return _navToPostListAsyncCommand;
            }
        }
        private async Task NavToPostListAsync()
        {
            // Implement your logic here.
            await _navigationService.Navigate<PostListViewModel>();
        }
        #endregion
```

