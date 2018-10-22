# Adding a simple Service

Create a folder in the `MvvmCrossDemo.Core` project and name it as Services. Then create a new interface called `IGreetingService.cs` into this folder to show the greeting text:

```csharp
namespace MvvmCrossDemo.Core.Services
{
    public interface IGreetingService
    {
        string GetGreetingText(string name);
    }
}
```

Then create an implementation for this interface in the same folder, and name it as `GreetingService.cs`, as shown below:

```csharp
namespace MvvmCrossDemo.Core.Services
{
    public class GreetingService: IGreetingService
    {
        public string GetGreetingText(string name)
        {
            return $"Hello World, {name}.";
        }
    }
}
```

Later I will use a real `REST` API service to show how to get the data from an online API.