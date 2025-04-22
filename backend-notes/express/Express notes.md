
# What is the app
`app` is sth like a program runs in a specific port.
You create it :
```js
const app = express()
```

then you bind some function to specific http requests:
```js
/* return note collection */
app.get('/api/notes', (request, response) => {
  response.json(notes)
})

/* delete a note resource */
app.delete('/api/notes/:id', (request, response) => {
  const id = request.params.id
  notes = notes.filter(note => note.id !== id)

  response.status(204).end()
})
```

then you make app to listen connections: 
```js
const PORT = 3001
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```



# Callback functions

Express.js provides HTTP method functions that correspond to all the standard RESTful API operation  :
```js
app.get()
app.post()
app.delete()
app.put()
app.patch()
```

These functions takes 2 parameter, the url and the callback :

```js

/* delete a note resource */
app.delete('/api/notes/:id', (request, response) => {
  const id = request.params.id
  notes = notes.filter(note => note.id !== id)

  response.status(204).end()
})
	```

this callback have 2 parameter, request is the object created from request and response is the response that is waiting to be invoked with a specific method of it that sends response.


## Using :id 

This is one of the most useful part of express.js.

Let say there is an get request incoming with this url:
`http://localhost:3001/api/notes/10`

Normally if you were using http library you should somehow parse that incoming url and make sure first it is valid, means it is really in proper format like /api/notes/10
then you should make sure that it is a resource url with just a number -10 in this case- is added after collection url.

and after that you would parse this url and get this 10 as an id and return the resource with this specific id.

But with express you can do just this :

```js
app.get('/api/notes/:id', (request,response) => {
	const noteId = request.params.id;
	response.json(notes.find(note=> note.id === noteId));
})
```

this automatically both validate your url and parse id for you as you specified it as a parameter of that url.


# Problems with incoming request 

Sometimes our backend server may seems not working as expected. it has high change of resulting from the impairments of incoming request. 

For example problems can occur with the VS REST client if you accidentally add an empty line between the top row and the row specifying the HTTP headers. In this situation, the REST client interprets this to mean that all headers are left empty, which leads to the backend server not knowing that the data it has received is in the JSON format.

Sometimes when you're debugging, you may want to find out what headers have been set in the HTTP request. One way of accomplishing this is through the [get](http://expressjs.com/en/4x/api.html#req.get) method of the _request_ object, that can be used for getting the value of a single header. The _request_ object also has the _headers_ property, that contains all of the headers of a specific request.

You will be able to spot this missing _Content-Type_ header if at some point in your code you print all of the request headers with the _console.log(request.headers)_ command.



# Proper Error Handling in Express: Why Status Codes Matter

When building APIs with Express.js, proper error handling and appropriate HTTP status codes are essential for creating robust, maintainable applications. In this post, I'll explain why returning the correct status code is crucial and how to implement proper error handling in your Express routes.

## The Problem with Returning Everything as 200 OK

Let's look at this example route handler:

```javascript
app.get('/api/persons/:id', (request, response) => {
    const personId = request.params.id;
    const person = persons.find(person => person.id === personId);
    
    console.log(person);
    response.json(person);
});
```

At first glance, this code might seem fine. It searches for a person by ID and returns the result as JSON. However, there's a critical issue: regardless of whether the person is found or not, this route always returns a 200 OK status code.

When `person` is undefined (meaning no person with that ID exists), Express simply sends an empty response with a 200 status code. This creates several problems:

1. **Misleading responses**: A 200 status code indicates success, but returning nothing for a resource that doesn't exist is not a success scenario.
    
2. **Client confusion**: Clients consuming your API have to check if the response body is empty, rather than relying on the status code.
    
3. **Debugging difficulties**: When troubleshooting, it's harder to identify missing resources.
    
## The Better Approach: Using Appropriate Status Codes

Here's the improved version:

```javascript
app.get('/api/persons/:id', (request, response) => {
    const personId = request.params.id;
    const person = persons.find(person => person.id === personId);
    
    if (!person) {
        response.status(404).end();
    } else {
        response.json(person);
    }
});
```

This version includes proper error handling:

1. If no person is found with the given ID, it returns a 404 Not Found status code.
2. Only if a person is found does it return the person data with a 200 OK status code.

## Why Status Codes Matter

HTTP status codes were designed to communicate the outcome of HTTP requests. Using them correctly offers numerous benefits:

### 1. Clear Communication

Status codes provide immediate feedback about what happened with a request. Common status codes include:

- 200 OK: The request succeeded
- 201 Created: A new resource was created
- 400 Bad Request: The client sent an invalid request
- 401 Unauthorized: Authentication is required
- 403 Forbidden: The client doesn't have permission
- 404 Not Found: The requested resource doesn't exist
- 500 Internal Server Error: Something went wrong on the server

### 2. Better Client Integration

Frontend applications and API consumers rely on status codes to determine how to proceed. For instance, a status code of 404 might trigger a "Resource not found" message, while a 500 might display a general error message.

### 3. Improved Debugging

When something goes wrong, appropriate status codes make it much easier to identify and fix issues.

## Beyond Simple Error Handling

For a more comprehensive approach, you might want to include error messages:

```javascript
app.get('/api/persons/:id', (request, response) => {
    const personId = request.params.id;
    const person = persons.find(person => person.id === personId);
    
    if (!person) {
        return response.status(404).json({ 
            error: 'Person not found' 
        });
    }
    
    response.json(person);
});
```

This not only returns the correct status code but also provides a helpful error message explaining what went wrong.

## Conclusion

Taking the time to implement proper error handling with appropriate status codes makes your API more predictable, easier to use, and simpler to maintain. Remember that HTTP status codes exist for a reason—they're a standardized way to communicate about the outcome of requests, and using them correctly is a mark of a well-designed API.

Next time you're building an Express application, take a moment to consider your error handling approach. Your future self (and anyone consuming your API) will thank you!

