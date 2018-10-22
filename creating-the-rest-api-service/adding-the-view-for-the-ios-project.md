# Adding the View for the iOS project

Create a new blank Storyboard and name it as `Post.storyboard` in the root folder of the `MvvmCrossDemo.iOS` project.

Drag a Table View Controller from the Toolbox and place it on the `Post.Storyboard`. Click the bottom bar of it and set the `Class` property and the `Storyboard ID` property to `PostListView`. Select the `Use Storyboard ID` checkbox. Then the `PostListView.cs` and the `PostListView.designer.cs` will be automatically generated. Move them to the `Views` folder and update their namespace from `Blank` to `MvvmCrossDemo.iOS.Views`. Change the parent class of it, as shown below:

```csharp
[MvxFromStoryboard("Post")]
public partial class FirstView : MvxViewController<PostListViewModel>
```

TODO...

