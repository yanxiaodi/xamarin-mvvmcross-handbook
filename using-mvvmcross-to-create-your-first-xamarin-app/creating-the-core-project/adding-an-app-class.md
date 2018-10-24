# Adding an App class

`MvvmCross` use an `App` class to take responsibility for registering custom object on the IoC container and start the `ViewModels`. Add an `App` class to the root folder and update the content like this:

```csharp
using MvvmCross.IoC;
using MvvmCross.ViewModels;
using MvvmCrossDemo.Core.Models;
using MvvmCrossDemo.Core.ViewModels;

namespace MvvmCrossDemo.Core
{
    public class App : MvxApplication
    {
        public override void Initialize()
        {
            CreatableTypes()
                .EndingWith("Service")
                .AsInterfaces()
                .RegisterAsLazySingleton();
            RegisterAppStart<FirstViewModel>();
            ModelMapper.Init();
        }

    }
}
```

The `App` class inherits from `MvxApplication` class which comes from `MvvmCross`. You can name it as you like but I recommend you call it `App`. In this class we do two important things, one is to register all the services as singleton pattern \(because usually, the names of these services end with “`Service`”\), another is to call a `ViewModel` named `FirstViewModel` to show the first view when the app starts, which I described in the previous section.

The code uses `Reflection` to find all classes in the `MvvmCrossDemo.Core` assembly which have a public constructor and with names ending in Service, then find their interfaces and register them as lazy singletons. Lazy means it will not be created until it is first to be used in the app lifecycle. Using `Service` as the name ending is a convention. You can change it, but it is highly recommended to follow the convention.

`MvvmCross` provides us with an IoC container to manage the implementation for all the services. With the registration, the `ViewModel` will get the instance of the interface automatically without initializing it through the constructor injection, which is a common method to implement `Dependency Injection` \(The other methods contain setter injection, method injection, etc.\), like this:

```csharp
public FirstViewModel(IGreetingService greetingService)
{
      _greetingService = greetingService;
}
```

`MvvmCross` also provides us with some other explicit manual methods to register the services, as shown below:

```csharp
Mvx.IoCProvider.RegisterSingleton<IGreetingService>(new GreetingService());
Mvx.IoCProvider.RegisterType<IGreetingService, GreetingService>();
```

You can choose one way you like. Now whenever you need to use an `IGreetingService` reference, the IoC container will create an `IGreetingService` instance automatically. You can also use this code to get an implementation explicitly:

```csharp
Mvx.IoCProvider.Resolve<IGreetingService>();
```

There are a lot of similar methods and overrides to register and resolve the interfaces and the implementations for different lifecycles, such as singleton, dynamic, or lazy construction.

If you are not familiar with DI\(`Dependency Injection`\) or IoC\(`Inversion of Control`\), you can find more details here: [https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-2.1](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-2.1). I strongly recommend that you know what `Dependency Injection` is, which is an essential pattern to build an application with the clean architecture.

To sum up this section, we have completed these steps:

* We created the Core project for all specific platforms based on .NET Standard.
* We created a service interface and the implementation of it.
* We created an App class to register the services and call the first `ViewModel`/`View`.
* We created one `ViewModel` and inject the service instance into it to complete the logic.
* We also added the code snippet to simplify the input.

Now we can add the specific platform projects.

