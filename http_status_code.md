# HTTP Status Code

### HTTP Status Codes

Every HTTP transaction has a status code sent back by the server to define how the server handled the transaction. Here is a list of the most common ones.

List of Common HTTP Status Codes

1. 200 OK
2. 300 Multiple Choices
3. 301 Moved Permanently
4. 302 Found
5. 304 Not Modified
6. 307 Temporary Redirect
7. 400 Bad Request
8. 401 Unauthorized
9. 403 Forbidden
10. 404 Not Found
11. 410 Gone
12. 500 Internal Server Error
13. 501 Not Implemented
14. 503 Service Unavailable
15. 550 Permission denied

### HTTP Status Code - 200 OK

The request has succeeded. The information returned with the response is dependent on the method used in the request.


### HTTP Status Code - 300 Multiple Choices

The requested resource has different choices and cannot be resolved into one. For example, there may be several index.html pages depending on which language is wanted (such as Dutch).

### HTTP Status Code - 301 Moved Permanently

The requested resource has been assigned a new permanent URI and any future references to this resource should use one of the returned URIs.


### HTTP Status Code - 302 Found

The requested resource resides temporarily under a different URI. Since the redirection might be altered on occasion, the client SHOULD continue to use the Request-URI for future requests.


### HTTP Status Code - 304 Not Modified

If the client has performed a conditional GET request and access is allowed, but the document has not been modified, the server SHOULD respond with this status code. The 304 response MUST NOT contain a message-body, and thus is always terminated by the first empty line after the header fields. If the client has done a conditional GET and access is allowed, but the document has not been modified since the date and time specified in If-Modified-Since field, the server responds with a 304 status code and does not send the document body to the client. Response headers are as if the client had sent a HEAD request, but limited to only those headers which make sense in this context. This means only headers that are relevant to cache managers and which may have changed independently of the document's Last-Modified date. Examples include Date , Server and Expires . The purpose of this feature is to allow efficient updates of local cache information (including relevant meta information) without requiring the overhead of multiple HTTP requests (e.g. a HEAD followed by a GET) and minimizing the transmittal of information already known by the requesting client (usually a caching proxy).


### HTTP Status Code - 307 Temporary Redirect

The requested resource resides temporarily under a different URI. Since the redirection MAY be altered on occasion, the client SHOULD continue to use the Request-URI for future requests. This response is only cacheable if indicated by a Cache-Control or Expires header field.


### HTTP Status Code - 400 Bad Request

The request could not be understood by the server due to malformed syntax. The client SHOULD NOT repeat the request without modifications.


### HTTP Status Code - 401 Unauthorized

The request requires user authentication. The response MUST include a WWW-Authenticate header field containing a challenge applicable to the requested resource.


### HTTP Status Code - 403 Forbidden

The server understood the request, but is refusing to fulfill it. Authorization will not help and the request SHOULD NOT be repeated.


### HTTP Status Code - 404 Not Found

The server has not found anything matching the Request-URI. No indication is given of whether the condition is temporary or permanent.


### HTTP Status Code - 410 Gone

The requested resource is no longer available at the server and no forwarding address is known. This condition is expected to be considered permanent. Clients with link editing capabilities SHOULD delete references to the Request-URI after user approval.

If the server does not know, or has no facility to determine, whether or not the condition is permanent, the status code 404 Not Found SHOULD be used instead. This response is cacheable unless indicated otherwise.


### HTTP Status Code - 500 Internal Server Error

The server encountered an unexpected condition which prevented it from fulfilling the request.


### HTTP Status Code - 501 Not Implemented

The server does not support the functionality required to fulfill the request. This is the appropriate response when the server does not recognize the request method and is not capable of supporting it for any resource.


### HTTP Status Code - 503 Service Unavailable

Your web server is unable to handle your HTTP request at the time. There are a myriad of reasons why this can occur but the most common are:

server crash
server maintenance
server overload
server maliciously being attacked
a website has used up its allotted bandwidth
server may be forbidden to return the requested document
This is usually a temporary condition. Since you are getting a return code, part of the server is working. The web people have made the server return this code until they fix the problem.

If you do not get service back soon, contact your web host as they would know the best. Some web hosts have server status pages you can check.


### HTTP Status Code - 550 Permission Denied

The server is stating the account you have currently logged in as does not have permission to perform the action you are attempting. You may be trying to upload to the wrong directory or trying to delete a file.

