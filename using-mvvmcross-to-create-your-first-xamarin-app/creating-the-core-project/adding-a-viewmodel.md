# Adding a ViewModel

In the `ViewModel`, we need to have some values that are used to bind to the controls on the UI. Now create a new folder called `ViewModels` and create a new class called `FirstViewModel.cs` which inherits from the `MvxViewModel` class. `MvxViewModel` is a base class to encapsulate some essential features of the `ViewModel`, such as the `INotifyPropertyChanged` interface and the lifecycle of the page.

`INotifyPropertyChanged` is the important interface when using data-binding, which can notify the clients that a property value has changed. You can get more details about this interface here: [https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.inotifypropertychanged?view=netframework-4.7.2](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.inotifypropertychanged?view=netframework-4.7.2) . If we implement this interface by ourselves, you might feel that it is a little bit tedious. With this base class - `MvxViewModel`, we can easily create the properties that supports data-binding. Also, it exposes necessary lifecycle events to allow us to inject the handler to the specific lifecycle events. I created a code snippet to simplify the property inputs. You can find it here: [https://github.com/yanxiaodi/MvvmCrossDemo/blob/dev/MvvmCrossDemo/Resources/CodeSnippets/mvvmCross.snippet](https://github.com/yanxiaodi/MvvmCrossDemo/blob/dev/MvvmCrossDemo/Resources/CodeSnippets/mvvmCross.snippet).

Download this file and import it into your Code Snippets Manager, or just place them in this folder: C:\Users\YourUserName\Documents\Visual Studio 2017\Code Snippets\Visual C\#\My Code Snippets\MvvmCross.

![](../../.gitbook/assets/image%20%283%29.png)

Now you can input the shortcut `mvxpropdp` for the properties:

![](../../.gitbook/assets/image%20%282%29.png)

When I set the value to the property, it will call the `SetProperty(ref _property, value)` method that comes from `MvxViewModel` to simplify the implementation of the `INotifyPropertyChanged` interface.

And `mvxcmd` for the commands:

![](../../.gitbook/assets/image%20%2832%29.png)

`IMvxCommand` is a build-in command interface to implement the `ICommand` interface, which can be binded to the UI controls to response the user behaviour, such as click event of the `Button`, or select event of the `ListView`. `ICommond` is mostly used in the `MVVM` applications to separate the tight coupling between the UI and the event handlers. Most of popular `MVVM` frameworks in .NET provide an implementation of `ICommand`, such as `IMvxCommand` in `MvvmCross`.

At last, the `ViewModel` looks like this:

```csharp
using MvvmCross.Commands;
using MvvmCross.ViewModels;
using MvvmCrossDemo.Core.Services;
namespace MvvmCrossDemo.Core.ViewModels
{
    public class FirstViewModel: MvxViewModel
    {
        private readonly IGreetingService _greetingService;
        public FirstViewModel(IGreetingService greetingService)
        {
            _greetingService = greetingService;
        }

        #region UserName;
        private string _userName;
        public string UserName
        {
            get => _userName;
            set => SetProperty(ref _userName, value);
        }
        #endregion

        #region Greeting;
        private string _greeting;
        public string Greeting
        {
            get => _greeting;
            set => SetProperty(ref _greeting, value);
        }
        #endregion


        #region GetGreetingCommand;
        private IMvxCommand _getGreetingCommand;
        public IMvxCommand GetGreetingCommand
        {
            get
            {
                _getGreetingCommand = _getGreetingCommand ?? new MvxCommand(GetGreeting);
                return _getGreetingCommand;
            }
        }
        private void GetGreeting()
        {
            // Implement your logic here.
            Greeting = _greetingService.GetGreetingText(UserName);
        }
        #endregion
    }
}
```

There are a couple of properties in the `ViewModel` to accept the input from the user and show the greeting message. We also have a command to respond the click event of a button. In the constructor method, there is a param called `greetingService`. I will inject the implementation of the interface in the next section.

