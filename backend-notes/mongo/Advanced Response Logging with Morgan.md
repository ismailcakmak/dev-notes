

In web development, logging is essential for debugging, monitoring, and understanding your application's behavior. While most logging solutions focus on incoming requests, capturing response data can be equally important. In this post, we'll explore how to log response objects using Morgan in Express.js applications, diving into some interesting JavaScript concepts along the way.

## The Challenge with Response Logging

Morgan is a popular HTTP request logger middleware for Node.js that makes it easy to log incoming requests. However, out of the box, Morgan doesn't provide a straightforward way to log response bodies. This is because of how Express.js processes middleware:

1. Request comes in
2. Middleware processes the request in order
3. Route handler generates a response
4. Response is sent back to the client

Morgan typically operates in step 2, but the response body isn't available until after step 3. So how do we solve this timing problem?

## The Solution: Middleware that Wraps Response Methods

To capture response data, we need to create custom middleware that wraps the `res.json()` method. This technique allows us to intercept the response body before it's sent to the client.

Here's how it works:

```javascript
app.use((req, res, next) => {
  // Store original res.json method
  const originalJson = res.json;
  
  // Override res.json method
  res.json = function(body) {
    // Store the response body for Morgan to access
    res.locals.responseBody = JSON.stringify(body);
    
    // Call the original method
    return originalJson.call(this, body);
  };
  
  next();
});

// Create a token for response body, will be populated by our custom middleware
morgan.token('response-body', (req, res) => {
  return res.locals.responseBody || '';
});

// Use the token in your Morgan format string
app.use(morgan(':method :url :status :response-time ms :response-body'));
```

This approach uses method overriding to:

1. Save the original `res.json` method
2. Replace it with our custom function that stores the response body
3. Call the original method to maintain normal functionality
4. Define a custom Morgan token that accesses our stored response body

## Understanding the JavaScript Magic: The `this` Context

One of the most interesting aspects of this solution is this line:

```javascript
return originalJson.call(this, body);
```

Why do we use `.call(this, body)` instead of simply `originalJson(body)`? This has to do with JavaScript's concept of execution context.

When we override a method like `res.json`, we need to ensure that when we call the original method, it still has access to the correct `this` context, which is the response object (`res`).

If we were to just call `originalJson(body)` directly, the function would lose its binding to the response object. Inside the function, `this` would no longer refer to `res` but would default to the global object (or undefined in strict mode). This would break the function, as it relies on accessing other properties and methods of the response object through `this`.

By using `.call(this, body)`, we explicitly set the execution context for the function call. The first argument to `.call()` becomes the value of `this` inside the function. Since our wrapper function is assigned as a method on the `res` object (`res.json = function() {...}`), when it's called as a method (`res.json(data)`), `this` naturally refers to `res`. So `originalJson.call(this, body)` effectively means "call the original JSON method with the response object as its context."

This ensures that the original function operates exactly as it would have if we hadn't intercepted it.

## Why This Approach Works

Our custom middleware function is assigned directly to `res.json`. When a function is assigned as a method on an object and then called through that object (like `res.json(someData)`), JavaScript automatically sets `this` to be the object the method was called on.

So when route handlers call `res.json(data)`:

1. Our interceptor function executes
2. Inside that function, `this` refers to the `res` object
3. We store the response body data in `res.locals.responseBody`
4. We call the original function with the correct context using `.call(this, body)`
5. Later, Morgan can access the stored response body when generating logs

## Practical Applications

This response logging technique is valuable for:

- Debugging API responses
- Auditing data being sent to clients
- Performance monitoring
- Security analysis
- Compliance requirements that need complete request/response logs

## Extending the Concept

You can apply this same pattern to other response methods like `res.send()`, `res.render()`, or any custom response methods your application uses. The key is always to:

1. Save the original method
2. Create a wrapper that captures the data you need
3. Call the original with the correct context
4. Make the captured data available to your logger

## Conclusion

Logging response objects with Morgan provides valuable insights into your application's behavior. While it requires a bit of middleware magic, understanding the JavaScript principles behind it—particularly how function context and the `this` keyword work—makes the solution elegant and maintainable.

By intercepting and wrapping response methods, we can capture data at exactly the right moment in the request lifecycle, all without disrupting the normal flow of our application.

Remember that logging response bodies may include sensitive information, so consider masking or filtering confidential data in production environments. Also, extensive logging can impact performance, so use this technique judiciously based on your application's needs.