# Implementing MasterDetail layout in Xamarin.Forms by MvvmCross

In my [Xamarin & MvvmCross Handbook](https://yanxiaodi.gitbook.io/xamarin-mvvmcross-handbook/), I demonstrated fundamentals to develop a basic Xamarin application with MvvmCross Framework. There are more details to consider when we develop a real application, such as the layout, the styles, and the database, etc. For example, Hamburger Menu layout is a very common navigation pattern in modern mobile applications. But actually, there is no default HamburgerMenu control in Xamarin. So we have to use MasterDetail navigation mode to implement the Hamburger Menu. Next, I'm going to show you how to implement a MasterDetail layout in the Xamarin.Forms application. Before we start, I recommend you to read the official documentation about MasterDetailPage here: [Xamarin.Forms Master-Detail Page](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/navigation/master-detail-page).

My dev environment is shown below:

* Windows 10 version 10.0.17134
* Visual Studio 2017 version 15.9.4
* Xamarin.Forms version 3.4.0.1008975
* MvvmCross version 6.2.2

Let's get started.