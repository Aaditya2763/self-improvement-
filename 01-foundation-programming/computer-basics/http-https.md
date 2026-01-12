# HTTP/HTTPS

## Learning Objectives
- Understand HTTP/HTTPS protocol
- Master request-response model
- Learn status codes
- Understand security

## Topics

### 1. HTTP Basics
- Hypertext Transfer Protocol
- Stateless protocol
- Request-response model
- HTTP versions (1.1, 2, 3)

### 2. HTTP Methods
- **GET**: Retrieve data
- **POST**: Create data
- **PUT**: Replace data
- **PATCH**: Partial update
- **DELETE**: Remove data
- **HEAD**: Like GET but no body
- **OPTIONS**: Describe communication options

### 3. Status Codes
- **1xx**: Informational
- **2xx**: Success (200, 201, 204)
- **3xx**: Redirection (301, 302, 304)
- **4xx**: Client error (400, 401, 403, 404)
- **5xx**: Server error (500, 502, 503)

### 4. Headers
**Request Headers**
- User-Agent
- Accept, Content-Type
- Authorization
- Cookie

**Response Headers**
- Content-Type
- Content-Length
- Set-Cookie
- Cache-Control

### 5. HTTPS
- SSL/TLS encryption
- Certificate authority
- Public-private key pair
- Man-in-the-middle prevention

## HTTP Request Structure
```
GET /api/users HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: application/json
Authorization: Bearer token123

(body for POST/PUT)
```

## HTTP Response Structure
```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 256
Set-Cookie: sessionId=xyz123

{
  "id": 1,
  "name": "John"
}
```

## Common Status Codes
```
200 OK              - Request successful
201 Created         - Resource created
204 No Content      - Success, no body
301 Moved Perm      - Permanent redirect
302 Found           - Temporary redirect
304 Not Modified    - Use cached version
400 Bad Request     - Invalid request
401 Unauthorized    - Authentication required
403 Forbidden       - Access denied
404 Not Found       - Resource not found
500 Server Error    - Server error
502 Bad Gateway     - Invalid response
503 Unavailable     - Service down
```

## HTTP vs HTTPS
| Feature | HTTP | HTTPS |
|---------|------|-------|
| Encryption | ❌ | ✅ |
| Certificate | ❌ | ✅ |
| Performance | Fast | Slightly slower |
| Port | 80 | 443 |
| Security | Low | High |

## Practice Tasks
- [ ] Make HTTP requests (curl, Postman)
- [ ] Understand status codes
- [ ] Set proper headers
- [ ] Handle redirects
- [ ] Use HTTPS properly

## Interview Tips
- Explain HTTP/HTTPS difference
- Know common status codes
- Understand request methods
- Know header purposes
- Explain SSL/TLS basics

## Tools
- Postman: API testing
- curl: Command-line requests
- Chrome DevTools: Network tab
- Wireshark: Network analysis

## Resources
- MDN: HTTP
- HTTP Status Codes Guide
- RFC 7231: HTTP Semantics
