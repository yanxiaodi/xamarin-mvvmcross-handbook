### Creating the ViewModel

Next, add a new class file called `MasterDetailViewModel` in the `ViewModels` folder in the MvxFormsMasterDetailDemo.Core project. Change it to inherit from `MvxViewModel` class. Usually, we also need to use the `NavigationService` to implement the navigation in the ViewModel. So use dependency injection to inject the instance of `IMvxNavigationService`:

```c#
using MvvmCross.Navigation;
using MvvmCross.ViewModels;

namespace MvxFormsMasterDetailDemo.Core.ViewModels
{
    public class MasterDetailViewModel : MvxViewModel
    {
        readonly IMvxNavigationService _navigationService;

        public MasterDetailViewModel(IMvxNavigationService navigationService)
        {
            _navigationService = navigationService;
        }
    }
}
```