# Creating the models

Come back to the `MvvmCrossDemo.Core` project. You might be familiar with how to create a `POCO` model. I prefer to create a `BaseResponseMessage` to encapsulate the real Response Model Message for the UI layer, so it can make it easier to deal with some unexpected response message, such as null, or the other `HTTP` Status.

Add a folder called `Models` to the root folder of the `MvvmCrossDemo.Core` project and create a new file called `BaseResponseMessage.cs` into the `Models` folder, which looks like this:

```csharp
namespace MvvmCrossDemo.Core.Models
{
    public class BaseResponseMessage
    {
        public bool IsSuccess { get; set; }
        public string Message { get; set; }
    }

    public class ResponseMessage<T> : BaseResponseMessage
    {
        public T Result { get; set; }
    }
}
```

The `BaseResponseMessage` is used to represent the result of some operations, such as `Delete`, because it contains no real entity in the response content. `ResponseMessage` will contain a real model entity so we can get it if the `IsSuccess` property is True, otherwise, we should show some messages to the users.

To deserialize the `JSON` data, we also need to add the reference to `Newtonsoft.Json` the NuGet Package Manager or input the command in the Package Manager Console:

```bash
Install-Package Newtonsoft.Json
```

Then we can add the real model class. Create a new class called `Post.cs` into the `Models` folder, which is shown as below:

```csharp
using Newtonsoft.Json;

namespace MvvmCrossDemo.Core.Models
{
    public class Post
    {
        [JsonProperty("userId")]
        public int UserId { get; set; }

        [JsonProperty("id")]
        public int Id { get; set; }

        [JsonProperty("title")]
        public string Title { get; set; }

        [JsonProperty("body")]
        public string Body { get; set; }
    }
}
```

It is a common function to serizlize an object to `JSON` string or deserialize a `JSON` string to an object. Also, when we send the requests to the APIs, we need to convert the object to `HttpContent`. we can create some extension methods to simplify this process. Create a new folder called `Infrastructure` into the root folder of the `MvvmCrossDemo.Core` project and create a folder named `Extensions` into the `Infrastructure` folder. Then add a new file called `JsonExtentions.cs` file in to the `Extensions` folder. Replace the default content with these codes:

```csharp
using Newtonsoft.Json;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;

namespace MvvmCrossDemo.Core.Infrastructure.Extensions
{
    public static class JsonExtensions
    {
        public static async Task<T> ReadAsJsonAsync<T>(this HttpResponseMessage response)
        {
            string jsonString = await response.Content.ReadAsStringAsync();
            return jsonString.ToObject<T>();
        }

        public static T ToObject<T>(this string jsonString)
        {
            return JsonConvert.DeserializeObject<T>(jsonString);
        }

        public static StringContent ToStringContent<T>(this T obj)
        {
            return ToStringContent(obj, "application/json");
        }

        public static StringContent ToStringContent<T>(this T obj, string contentType)
        {
            string json = JsonConvert.SerializeObject(obj);
            return new StringContent(json, Encoding.UTF8, contentType);
        }
    }
}

```

