# Creating the Forms.UI project

Create a Class Library\(.NET Standard\) project which is called `MvvmCrossDemo.Form.UI` by Visual Studio:

![](../../.gitbook/assets/image%20%2845%29.png)

Delete the default `Class1.cs` file. Install the `Xamarin.Forms` package from the NuGet Package Manager, like this:

![](../../.gitbook/assets/image%20%2851%29.png)

Or input the command below in the NuGet Manager Console:

```bash
Install-Package Xamarin.Forms
```

Then install `MvvmCross.Forms` package by the NuGet Package Manager:

![](../../.gitbook/assets/image%20%2836%29.png)

You can also input the command below in your Package Manger Console:

```bash
Install-Package MvvmCross.Forms
```

As we mentioned before, all the UI components will be hosted in this UI project. To achieve the reusability, add the reference to the `MvvmCrossDemo.Core` project that we created before, like this:

![](../../.gitbook/assets/image%20%2848%29.png)

