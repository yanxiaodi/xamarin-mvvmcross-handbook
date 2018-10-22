# Adding the App.xaml and the App.xaml.cs

Create a new Xamarin.Forms Content Page item into the `MvvmCross.Forms.UI` project. Notice that do not select the other file items, as shown below:

![](../../.gitbook/assets/image%20%2811%29.png)

Open this file and replace the content with the below codes:

```markup
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="MvvmCrossDemo.Forms.UI.App">
    <Application.Resources>
    </Application.Resources>
</Application>
```

You can find the sample code here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/FormsUIContent/App.xaml.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/FormsUIContent/App.xaml.pp).

Open the `App.xaml.cs` file and update the content. The default base class of the `App` class is `ContentPage`, and we need to change it to inherit from the `Xamarin.Forms.Application` class, like this:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace MvvmCrossDemo.Forms.UI
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
        }
    }
}
```

You can find the sample code here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/FormsUIContent/App.xaml.cs.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Forms/FormsUIContent/App.xaml.cs.pp).

