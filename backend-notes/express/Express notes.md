
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


## About response lifecycle and middleware execution

endpoint içersisinde kullandığımız her funksiyon response'ı ateşlemez. 

Örnek : 
```js
app.get('/test', (req, res) => {
  res.status(200);        // Just sets status, doesn't send
  res.set('X-Custom', 'value'); // Just sets header, doesn't send
  res.type('json');       // Just sets content-type, doesn't send
  
  console.log('This will run'); // ✅ This executes
  
  res.json({ message: 'hello' }); // This actually SENDS the response
  
  console.log('This will NOT run'); // ❌ This won't execute
});
```
### Methods that DO send the response immediately:
res.json()
res.send()
res.end()
res.redirect()
res.render()

When a response-sending methods executed they don't wait for the end of the file or for other middleware. Once sent, the HTTP response is complete and no further middleware in the chain will execute.

Once you use a response-sending method, the response is sent and the middleware chain stops. Any middleware that comes after won't execute.

so, key takeaway: If you want middleware to process responses, it must either:

- Run before the response is sent (to intercept/modify)
- Override response methods to catch when they're called
- Be error middleware (which runs after errors occur)

Examples of that : 


- run before response is sent : 

```js
// Middleware that adds security headers to all responses
app.use((req, res, next) => {
  res.set({
    'X-Frame-Options': 'DENY',
    'X-Content-Type-Options': 'nosniff',
    'X-XSS-Protection': '1; mode=block'
  });
  next(); // Continue to route handler
});

// Middleware that logs request info before response
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
  next(); // Continue to route handler
});

app.get('/users', (req, res) => {
  res.json({ users: ['Alice', 'Bob'] }); // Headers already set by middleware above
});
```


- override response method : 

```js
// Middleware that wraps all JSON responses with metadata
app.use((req, res, next) => {
  const originalJson = res.json;
  
  res.json = function(data) {
    console.log('JSON response intercepted:', data);
    
    // Wrap response with additional metadata
    const wrappedResponse = {
      success: true,
      timestamp: Date.now(),
      path: req.originalUrl,
      data: data
    };
    
    return originalJson.call(this, wrappedResponse);
  };
  
  next();
});

// Middleware that logs all outgoing responses
app.use((req, res, next) => {
  const originalSend = res.send;
  const originalJson = res.json;
  
  res.send = function(body) {
    console.log(`Response sent: ${res.statusCode} - Body:`, body);
    return originalSend.call(this, body);
  };
  
  res.json = function(obj) {
    console.log(`JSON sent: ${res.statusCode} - Data:`, obj);
    return originalJson.call(this, obj);
  };
  
  next();
});

app.get('/products', (req, res) => {
  res.json({ products: ['laptop', 'phone'] }); // Gets wrapped and logged
});

app.get('/text', (req, res) => {
  res.send('Hello World'); // Gets logged
});
```

- error middleware :

```js
// Regular route that might throw an error
app.get('/divide', (req, res) => {
  const { a, b } = req.query;
  
  if (b === '0') {
    throw new Error('Division by zero!');
    // This line never executes
    res.json({ result: 'never sent' });
  }
  
  res.json({ result: a / b });
});

// Another route that might have an async error
app.get('/async-error', async (req, res, next) => {
  try {
    // Simulate async operation that fails
    await new Promise((resolve, reject) => {
      setTimeout(() => reject(new Error('Async operation failed')), 1000);
    });
    
    res.json({ message: 'success' }); // Never reached
  } catch (error) {
    next(error); // Pass error to error middleware
  }
});

// Error middleware - MUST have 4 parameters (err, req, res, next)
app.use((err, req, res, next) => {
  console.error('Error caught by middleware:', err.message);
  console.error('Stack:', err.stack);
  
  // Log error details
  console.log(`Error on ${req.method} ${req.originalUrl}`);
  
  // Send error response
  res.status(500).json({
    error: 'Internal Server Error',
    message: err.message,
    timestamp: new Date().toISOString()
  });
});

// 404 handler (runs when no routes match)
app.use('*', (req, res) => {
  res.status(404).json({
    error: 'Not Found',
    message: `Route ${req.originalUrl} not found`,
    timestamp: new Date().toISOString()
  });
});
```



Real world combined example :
```js
const express = require('express');
const app = express();

// 1. BEFORE middleware - Add CORS headers
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Headers', 'Content-Type');
  next();
});

// 2. OVERRIDE middleware - Add response timing
app.use((req, res, next) => {
  const start = Date.now();
  const originalJson = res.json;
  
  res.json = function(data) {
    const duration = Date.now() - start;
    console.log(`Request took ${duration}ms`);
    
    // Add timing to response
    data._meta = { responseTime: `${duration}ms` };
    return originalJson.call(this, data);
  };
  
  next();
});

// Routes
app.get('/api/users', (req, res) => {
  res.json({ users: ['Alice', 'Bob'] }); // Gets timing added + CORS headers
});

app.get('/api/error', (req, res) => {
  throw new Error('Something broke!'); // Goes to error middleware
});

// 3. ERROR middleware - Handle all errors
app.use((err, req, res, next) => {
  console.error(`Error: ${err.message}`);
  res.status(500).json({
    error: 'Server Error',
    message: err.message
  });
});

app.listen(3000);
```

## Conclusion

Taking the time to implement proper error handling with appropriate status codes makes your API more predictable, easier to use, and simpler to maintain. Remember that HTTP status codes exist for a reason—they're a standardized way to communicate about the outcome of requests, and using them correctly is a mark of a well-designed API.

Next time you're building an Express application, take a moment to consider your error handling approach. Your future self (and anyone consuming your API) will thank you!

# Express 
Express is just an npm package - a JavaScript library that runs on top of Node.js. Node.js does the actual work:

- Creates the HTTP server
- Listens on the port
- Handles incoming requests
- Manages the event loop
- Provides the runtime environment

Express is just a framework that:

- Wraps Node.js's built-in http module
- Provides a cleaner, more organized API
- Adds routing, middleware, and other conveniences
- Simplifies request/response handling


You could build a web server using just Node.js's built-in modules, but Express makes it much easier by providing:

- Simplified routing (app.get(), app.post())
- Middleware system
- Request/response helpers
- Template engine support