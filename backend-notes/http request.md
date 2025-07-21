
# what is http requests 
HTTP requests are indeed electrical signals that get converted to text by computer hardware.


Every HTTP request has this anatomy:
```
[METHOD] [PATH] [HTTP_VERSION]
[HEADERS]
[BLANK LINE]
[BODY]
```

HTTP requests are indeed electrical signals that get converted to text, but let me help you understand their complete anatomy. They're actually structured text messages (not necessarily JSON) that follow a specific format.

## Basic Structure

Every HTTP request has this anatomy:

```
[METHOD] [PATH] [HTTP_VERSION]
[HEADERS]
[BLANK LINE]
[BODY]
```

## 1. Request Line
The first line contains three parts:
- **Method**: What you want to do (GET, POST, PUT, DELETE, etc.)
- **Path**: Where you want to go (/users, /api/data, etc.)
- **HTTP Version**: Usually HTTP/1.1 or HTTP/2

Example: `GET /users/123 HTTP/1.1`

## 2. Headers
Key-value pairs that provide metadata about the request. eaders are always key-value pairs separated by colons, never JSON. Each header is on its own line:

```
Host: api.example.com
Content-Type: application/json
Authorization: Bearer abc123
User-Agent: Mozilla/5.0...
Accept: application/json
```

## 3. Blank Line
A mandatory empty line that separates headers from the body.

## 4. Body (Optional)
The actual data you're sending. This could be:
- JSON data
- Form data
- Files
- Plain text
- Nothing (for GET requests)

## Complete Example

Here's what a real HTTP request looks like:

```
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 45
Authorization: Bearer xyz789

{"name": "John", "email": "john@example.com"}
```

The key thing to understand: the Content-Type header tells the server how to interpret the body data. Whether it's JSON, plain text, binary image data, or form fields - it's all just bytes being transmitted, but the Content-Type header gives context for parsing it correctly.

# Different http request examples 

## 1. Plain Text Body

```
POST /api/notes HTTP/1.1
Host: example.com
Content-Type: text/plain
Content-Length: 23

This is just plain text.
```

## 2. Form Data (URL-encoded)

```
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 29

username=john&password=secret
```

## 3. Multipart Form Data (with file upload)

```
POST /upload HTTP/1.1
Host: example.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Length: 348

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="description"

My vacation photo
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="photo"; filename="beach.jpg"
Content-Type: image/jpeg

[BINARY IMAGE DATA GOES HERE - actual bytes of the JPG file]
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

## 4. Raw Binary Data (sending a JPG directly)

```
POST /api/images HTTP/1.1
Host: example.com
Content-Type: image/jpeg
Content-Length: 87452

[BINARY JPG DATA - this would be the actual bytes of the image file]
```

## 5. XML Body

```
POST /api/soap HTTP/1.1
Host: example.com
Content-Type: text/xml
Content-Length: 156

<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <getUserInfo>
      <userId>123</userId>
    </getUserInfo>
  </soap:Body>
</soap:Envelope>
```

## 6. No Body (GET request)

```
GET /api/users/123 HTTP/1.1
Host: example.com
Authorization: Bearer abc123
Accept: application/json

[No body - just ends after the blank line]
```




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