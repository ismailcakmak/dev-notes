
Common status codes:

- 200 OK: The request succeeded
- 201 Created: A new resource was created
- 400 Bad Request: The client sent an invalid request
- 401 Unauthorized: Authentication is required
- 403 Forbidden: The client doesn't have permission
- 404 Not Found: The requested resource doesn't exist
- 500 Internal Server Error: Something went wrong on the server




Örneğin client şunu post request olarak gönderirse : 
```
{
    "name" : ,
    "number" : "552 232 8344"
  }
```

burdan express 400 - bad request dönderir çünkü bu invalid bir json formatı

fakat şu bir invalid bir request olmaz :
```
  {
    "name" : "",
    "number" : "552 232 8344"
  }
```