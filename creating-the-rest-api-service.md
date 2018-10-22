# Creating the REST API Service

I am going to show how to call a `REST` API to get the data for the `ViewModel`. There is a great fake online `REST` API for testing and prototyping: [https://jsonplaceholder.typicode.com](https://jsonplaceholder.typicode.com). For example, if we call this URL: [https://jsonplaceholder.typicode.com/posts](https://jsonplaceholder.typicode.com/posts), we will get the data like this:

```javascript
[
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
    "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
  },
  {
    "userId": 1,
    "id": 2,
    "title": "qui est esse",
    "body": "est rerum tempore vitae\nsequi sint nihil reprehenderit dolor beatae ea dolores neque\nfugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis\nqui aperiam non debitis possimus qui neque nisi nulla"
  }
]
```

