# Adding the View

Create a new folder named `Views` into the `MvvmCrossDemo.Forms.UI` project to store the views. Add a new Xamarin.Forms Content Page item called `FirstView.xaml` into the View folder to correspond the `FirstViewModel.cs` in the `MvvmCrossDemo.Core` project so that we can reuse the `ViewModel` layer.

`Xamarin.Forms` uses the `XAML` language to describe the UI but bear in mind that there are slight differences between the `Xamarin.Forms` XAML and the UWP XAML. For instance, we use `TextBox` in UWP to represent a control that can receive the input from users, but in Xamarin.Forms, the control is called `Entry`. For the `Button` control, in UWP we use `Content` property to show the text of the `Button`, but in `Xamarin.Forms`, the property is called `Text`. It makes a little bit inconvenience but probably, they will be unified to `XAML Standard` in the future. For more details, please check all the control definitions of `Xamarin.Forms` here: [https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/).

Open the `FirstView.xaml.cs` and make sure it is inherited from `MvxContentPage`, as shown below:

```csharp
using MvvmCross.Forms.Views;
using MvvmCrossDemo.Core.ViewModels;

namespace MvvmCrossDemo.Forms.UI.Views
{
    public partial class FirstView : MvxContentPage<FirstViewModel>
    {
        public FirstView()
        {
            InitializeComponent();
        }
    }
}
```

Then open the `FirstView.xaml` file and update the content like this:

```csharp
<?xml version="1.0" encoding="utf-8" ?>
<views:MvxContentPage x:TypeArguments="viewModels:FirstViewModel"
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:views="clr-namespace:MvvmCross.Forms.Views;assembly=MvvmCross.Forms"
    xmlns:mvx="clr-namespace:MvvmCross.Forms.Bindings;assembly=MvvmCross.Forms"
    xmlns:viewModels="clr-namespace:MvvmCrossDemo.Core.ViewModels;assembly=MvvmCrossDemo.Core"
    x:Class="MvvmCrossDemo.Forms.UI.Views.FirstView">
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Your Name"></Label>
            <Entry Text="{Binding UserName, Mode=TwoWay}"></Entry>
            <Button Text="Click Me!" Command="{Binding GetGreetingCommand}"></Button>
            <Label Text="{Binding Greeting}"></Label>
        </StackLayout>
    </ContentPage.Content>
</views:MvxContentPage> 
```

Look, the file content contains similar controls with UWP, but has some different controls. We changed the base class of the `FirstView` from `ContentPage` to `MvxContentPage` and added the references to `views`, `mvx` and `viewModels`. Additionally, we need to specify the `x:TypeArguments` as in the xaml file. The `MvxContentPage` class in the `FirstView.xaml.cs` also has a generic parameter as `FirstViewModel`. So that we built the relationship between the `FirstView` and the `FirstViewModel`. The first UI is done!

