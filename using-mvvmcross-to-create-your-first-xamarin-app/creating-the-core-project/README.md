# Creating the Core project

First, let us create a Core project for all the platform-specific projects. Open your VS 2017 and create a new solution called `MvvmCrossDemo`. Then Add a new .NET Standard 2.0 project and name it as `MvvmCrossDemo.Core`. We are going to place all shared codes here, including the ViewModels, the Services, and the Models. You can also divide the Core function into some specific projects to make the architecture more clear, for example, create another project for Models, such as `MvvmCrossDemo.Models`. But for this simple sample, one Core project is enough.

![](../../.gitbook/assets/image%20%2821%29.png)

Delete the auto-generated class like Class.cs file. Then install the `MvvmCross` package from the NuGet Package Manager:

![](../../.gitbook/assets/image%20%2850%29.png)

Or input the command in the Package Manager Console:

```bash
Install-Package MvvmCross
```

![](../../.gitbook/assets/image%20%2846%29.png)

For simplicity, we start from a simple View – HelloWorld. I am going to show a text “Hello World” on the page. `MvvmCross` offers initial sample files in its GitHub Repos. You can find them here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/).

