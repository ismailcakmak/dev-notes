
# Rest

The original definition of REST is not even limited to web applications. 
Instead, we take a more narrow view by only concerning ourselves with RESTful thinking and how RESTful APIs are typically understood in web applications.  

In Restful thinking, we have 2 terms resource and collection. Collecion containes resources.

So for example, let's assume that the root URL of our service is _www.example.com/api

In that case, this will be the collection url :  _www.example.com/api/notes

and every note will be the resource, therefore this will be an example resource url :
_www.example.com/api/notes/10

And those are the specific http requests to satisfy some certain operation on resource and collections : 

|URL|verb|functionality|
|---|---|---|
|notes/10|GET|fetches a single resource|
|notes|GET|fetches all resources in the collection|
|notes|POST|creates a new resource based on the request data|
|notes/10|DELETE|removes the identified resource|
|notes/10|PUT|replaces the entire identified resource with the request data|
|notes/10|PATCH|replaces a part of the identified resource with the request data|


## Common PAtterns
GET: Read data
POST: Create new resources (server assigns ID)
PUT: Create/replace entire resource (client specifies location)
PATCH: Update parts of a resource
DELETE: Remove a resource
HEAD: Check resource metadata
OPTIONS: Discover available methods
