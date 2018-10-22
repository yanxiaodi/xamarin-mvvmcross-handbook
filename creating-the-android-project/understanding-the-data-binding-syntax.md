# Understanding the data-binding syntax

Notice that we added a namespace in the `LinearLayout` node:

```text
xmlns:local="http://schemas.android.com/apk/res-auto"
```

For every control that needs to use data-binding, we use the syntax like this:

```text
local:MvxBind="Text UserName"
```

That means, the data-binding mechanism will try to find the `UserName` property in the data context that is an instance of a `FirstViewModel` \(but we did not set it at the moment\), then bind it to the `Text` property of the control. The data-binding is `Two-Way`, which is different with UWP. The default binding mode of UWP is `One-Way`. To get more details of Data-Binding for `MvvmCross`, please read this article: [https://www.mvvmcross.com/documentation/fundamentals/data-binding](https://www.mvvmcross.com/documentation/fundamentals/data-binding).

