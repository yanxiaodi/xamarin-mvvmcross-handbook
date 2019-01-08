### Creating the ViewModel

Create a `MenuViewModel` class in the `ViewModels` folder in the MvxFormsMasterDetailDemo.Core project. Replace the content with the following code:

```c#
using System.Collections.ObjectModel;
using MvvmCross.Navigation;
using MvvmCross.ViewModels;

namespace MvxFormsMasterDetailDemo.Core.ViewModels
{
    public class MenuViewModel : MvxViewModel
    {
        readonly IMvxNavigationService _navigationService;

        public MenuViewModel(IMvxNavigationService navigationService)
        {
            _navigationService = navigationService;
            MenuItemList = new MvxObservableCollection<string>()
            {
                "Contacts",
                "Todo"
            };
        }

        #region MenuItemList;
        private ObservableCollection<string> _menuItemList;
        public ObservableCollection<string> MenuItemList
        {
            get => _menuItemList;
            set => SetProperty(ref _menuItemList, value);
        }
        #endregion
    }
}

```

It has a `MenuItemList` property to store some menu items. For the simplicity, there are only two strings: `Contacts` and `Todo`. We also need to inject the instance of `IMvxNavigationService` in the constructor.