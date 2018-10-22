# Summary

Regarding the `Xamarin.Forms` demo, we stop here. You can continue to build the `PostList` View and the `PostDetail` View, etc. That is easy, isn’t it? For the `Xamarin.Forms`, we can benefit from the existed resources so that we just need to create the UI by `XAML` for all platforms and config the target projects to use the logic in the `MvvmCrossDemo.Core` project and the UI hosted in the `MvvmCrossDemo.Forms.UI` project. This approach can help you maximize the code-sharing of both the UI and the logic, especially if you are an experienced .NET developer \(with `XAML` background\).

But what I need to emphasize here is that `Xamarin.Forms` is not the silver bullet for cross-platform development. You ought to think carefully about your real scenario. If you need to fine tune the UI styles or call a big deal of complex native hardware functionalities \(including specific-platforms APIs\) targeting the specific platforms, probably the Xamarin Native is the better choice.

