# Navigation

We have finished several simple functionalities such as data-binding, command-binding, DI, etc. I am going to add another page to show the Post detail. When the user clicks one item in the post list, the App will navigate to the the detail view and show the post content.

Most of `MVVM` frameworks can provide us with a kind of attraction for the navigation to simplify navigating between views. Because we need to implement the navigation in the `ViewModel` layer which is shared by specific-platforms projects. Also, we need to transfer objects as the parameter between different views. Let us see how to navigate by `MvvmCross`.

