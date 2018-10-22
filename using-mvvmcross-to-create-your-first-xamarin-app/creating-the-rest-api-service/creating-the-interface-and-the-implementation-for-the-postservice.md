# Creating the interface and the implementation for the PostService

Create a new interface called `IPostService.cs` into the `Services` folder in the `MvvmCrossDemo.Core` project. Add two methods to it, as shown below:

```csharp
namespace MvvmCrossDemo.Core.Services
{
    public interface IPostService
    {
        Task<ResponseMessage<List<Post>>> GetPostList();
        Task<ResponseMessage<Post>> GetPost(int id);
    }
}
```

Create a new implementation class called `PostService.cs` in the same folder like this:

```csharp
using System;
using MvvmCrossDemo.Core.Infrastructure.Extensions;
using MvvmCrossDemo.Core.Models;
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;

namespace MvvmCrossDemo.Core.Services
{
    public class PostService : IPostService
    {
        private readonly HttpClient _httpClient;
        private string apiUrl = "https://jsonplaceholder.typicode.com/";

        public PostService()
        {
            _httpClient = new HttpClient();
        }

        public async Task<ResponseMessage<List<Post>>> GetPostList()
        {
            try
            {
                var response = await _httpClient.GetAsync($"{apiUrl}posts");
                if (response.StatusCode == System.Net.HttpStatusCode.OK)
                {
                    var result = await response.ReadAsJsonAsync<List<Post>>();
                    return new ResponseMessage<List<Post>>
                    {
                        IsSuccess = true,
                        Result = result
                    };
                }
                else
                {
                    return new ResponseMessage<List<Post>>
                    {
                        IsSuccess = false,
                        // Show the detailed error message here according to the response.
                        Message = "Errors"
                    };
                }
            }
            catch (Exception e)
            {
                // TODO: Log the exception here.
                return new ResponseMessage<List<Post>>
                {
                    IsSuccess = false,
                    // Show the detailed error message here.
                    Message = "Errors"
                };
            }
            
        }
        public async Task<ResponseMessage<Post>> GetPost(int id)
        {
            try
            {
                var response = await _httpClient.GetAsync($"{apiUrl}posts/{id}");
                if (response.StatusCode == System.Net.HttpStatusCode.OK)
                {
                    var result = await response.ReadAsJsonAsync<Post>();
                    return new ResponseMessage<Post>
                    {
                        IsSuccess = true,
                        Result = result
                    };
                }
                else
                {
                    return new ResponseMessage<Post>
                    {
                        IsSuccess = false,
                        // Show the detailed error message here according to the response.
                        Message = "Errors"
                    };
                }
            }
            catch (Exception e)
            {
                // TODO: Log the exception here.
                return new ResponseMessage<Post>
                {
                    IsSuccess = false,
                    // Show the detailed error message here.
                    Message = "Errors"
                };
            }
        }
    }
}
```

In the `PostService` class, I tried to handle all exceptions of the HTTP requests to avoid passing any unexpected result to the caller method. It is helpful to improve the stability of the App. We also need to use a logger to record the error message if you are developing a real App.

We do not need to worry about how to register the implement instance because `MvvmCross` will do everything by the code we mentioned before in the `App.cs`, as long as we follow the name convention of the services:

```csharp
CreatableTypes().EndingWith("Service").AsInterfaces().RegisterAsLazySingleton();
```

