### Creating the ViewModels

Add two new files named as `ContactsViewModel` and `TodoViewModel`  in the `ViewModels` folder of MvxFormsMasterDetailDemo.Core project. Make them to inherit from `MvxViewModel` class respectively:

```c#
using MvvmCross.ViewModels;

namespace MvxFormsMasterDetailDemo.Core.ViewModels
{
    public class ContactsViewModel : MvxViewModel
    {
    }
}
```

```c#
using MvvmCross.ViewModels;

namespace MvxFormsMasterDetailDemo.Core.ViewModels
{
    public class TodoViewModel : MvxViewModel
    {
    }
}
```