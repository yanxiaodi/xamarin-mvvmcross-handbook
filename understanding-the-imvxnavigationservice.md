# Understanding the IMvxNavigationService

`IMvxNavigationService` is an abstraction for the native navigation. Actually, it uses `ViewModel` to navigate, not from View to View. That means that we do not need to use platform-specific navigation, and `MvvmCross` will find the correct View to show. 

`IMvxNavigationService` supports a lot of different `async` methods to implement navigation. You can find all the interfaces here: [https://github.com/MvvmCross/MvvmCross/blob/develop/MvvmCross/Navigation/IMvxNavigationService.cs](https://github.com/MvvmCross/MvvmCross/blob/develop/MvvmCross/Navigation/IMvxNavigationService.cs). Get started from a simple one:

```csharp
Task Navigate<TViewModel>(IMvxBundle presentationBundle = null) where TViewModel : IMvxViewModel;
```

By this method, we can navigate to a new `ViewModel` without specifying the View we want to navigate to. `MvvmCross` will find the right view which is associated with this `ViewModel`.

If we need to pass a param to the new `ViewModel`, there is another method to do this:

```csharp
Task Navigate<TViewModel, TParameter>(TParameter param, IMvxBundle presentationBundle = null) where TViewModel : IMvxViewModel<TParameter>;
```

Before you use this method, make sure the target `ViewModel` has a generic param, like `PostDetailViewModel`:

```csharp
public class PostDetailViewModel : MvxViewModel<Post>
{
	…
}
```

For some situation, we need to get the result of the target `ViewModel` and return it to the source `ViewModel`. For example, if we edit a Post, we should update the post in the `PostListView` after returning.

