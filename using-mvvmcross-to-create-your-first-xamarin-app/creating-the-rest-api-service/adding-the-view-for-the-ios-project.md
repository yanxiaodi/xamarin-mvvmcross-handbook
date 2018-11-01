# Adding the View for the iOS project

Create a new blank Storyboard and name it as `PostList.storyboard` in the `Views` folder of the `MvvmCrossDemo.iOS` project, , and drag a `Button` to the page from the Toolbox:

![](../../.gitbook/assets/image%20%2850%29.png)

Set the `Name` of the `Button` as `btnNavToPostList`. Then update the `FirstView.cs` in `Views` folder to bind the command to the `Button` by adding the code below into the `ViewDidLoad` method:

```csharp
set.Bind(btnNavToPostList).To(vm => vm.NavToPostListAsyncCommand);
```

In the native iOS development, you can define a transition called `segue` to navigate from view A to view B in one storyboard but for `MvvmCross`, we need to unify the navigation pattern for all the platforms. So I recommend that it is better to use a one-ViewController-per-storyboard approach if you are using storyboards in `MvvmCross`.

Create a new Blank Storyboard and name it as `PostListView.storyboard` in the `Views` folder of the `MvvmCrossDemo.iOS` project. Drag a Table View Controller from the Toolbox and place it on the `PostListView.Storyboard`. Click the bottom bar of it and set the `Class` property and the `Storyboard ID` property to `PostListView`. Do not forget to select the Use Storyboard ID checkbox. Then the `PostListView.cs` and the `PostListView.designer.cs` will be automatically generated. Move them to the `Views` folder and update their namespace to `MvvmCrossDemo.iOS.Views`. Change the parent class of it, as shown below:

```csharp
    [MvxFromStoryboard("PostListView")]
    public partial class PostListView : MvxTableViewController<PostListViewModel>
    {
        public PostListView (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad()
        {
            base.ViewDidLoad();
            var source = new MvxStandardTableViewSource(TableView, 
                UITableViewCellStyle.Subtitle, 
                new NSString("PostItemViewCell"), "TitleText Post.Title; DetailText Post.Body");
            TableView.Source = source;

            var set = this.CreateBindingSet<PostListView, PostListViewModel>();
            set.Bind(source).To(vm => vm.PostList);
            set.Apply();
            TableView.ReloadData();
        }
}

```

`MvxTableViewController` is another build-in `ViewController` to present a standard iPhone table. In this view controller, we created a `MvxStandardTableViewSource` and bind it to the `TableView` in the `TableViewController`. The `Table` in iOS has four built-in cell styles which support the most common data displays. Here we use `Subtitle`. You can bind the `TitleText` and `DetailText` of the item in the list. At last, use this code to reload data:

```csharp
TableView.ReloadData();
```

We can extend the `MvxTableViewSource` class to customize the cell styles. For more details, please read this article: [https://www.mvvmcross.com/documentation/platform/ios/ios-tables-and-cells](https://www.mvvmcross.com/documentation/platform/ios/ios-tables-and-cells) . The result is:

![](../../.gitbook/assets/image%20%2838%29.png)

