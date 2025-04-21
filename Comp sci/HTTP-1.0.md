Documentation source: [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110.html)

##  Terminology and Core Concepts

### Resources
Resource is the target of an HTTP request and is identified by a URI (Uniform Resource Identifier).

### Representations
Representation is the format of the data that is sent in response to a request.

### Connections, Clients, and Servers
- **Connection**: A reliable transport or session layer over which a request and response are exchanged.
- **Client**: A program that initiates a connection to a server.
- **Server**: A program that accepts a connection from a client.

### Messages
- **Request**: A message sent from a client to a server, requires a method and request target.
- **Response**: A message sent from a server to a client.

### User Agents
A program that initiates a request to a server.  
Most commonly known user agents are web browsers, but there are many others like:
- spiders
- CLI tools
- billboard screens
- light bulbs
- firmware update scripts
- etc.

### Origin Server
A program that can originate an authoritative response to a request for a given target resource.  
Similar to user agents, origin servers are not only traditional servers but can include:
- home automation units
- autonomous robots
- traffic cameras
- etc.

### Intermediaries
A program that acts as a middleman between a client and a server.
#### Intermediary Types:
- **Proxy**: A message-forwarding agent that acts on behalf of a client.
- **Gateway**: Also known as a reverse proxy, a message forwarding agent that acts on behalf of an origin server.
- **Tunnel**: A message forwarding agent that acts on behalf of both a client and an origin server.

Illustration of intermediaries in a request-response cycle:
		 ```
```
         >             >             >             >
    UA =========== A =========== B =========== C =========== O
               <             <             <             <
```
### Caches
A "cache" is a local store of previous response messages and the subsystem that controls its message storage, retrieval, and deletion. A cache stores cacheable responses in order to reduce the response time and network bandwidth consumption on future, equivalent requests.

Diagram of a cached request:

```
            >             >
       UA =========== A =========== B - - - - - - C - - - - - - O
                  <             <
```

A cached request and a normal request is indistinguishable for the User Agent / client. 

### Example Message Exchange

The following example illustrates typical http/1.1 exchange for GET request on URI "http://www.example.com/hello.txt"

**Client request**
```
GET /hello.txt HTTP/1.1
User-Agent: curl/7.64.1
Host: www.example.com
Accept-Language: en, mi
```

**Server response**
```
HTTP/1.1 200 OK
Date: Mon, 27 Jul 2009 12:28:53 GMT
Server: Apache
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
ETag: "34aa387-d-1568eb00"
Accept-Ranges: bytes
Content-Length: 51
Vary: Accept-Encoding
Content-Type: text/plain

Hello World! My content includes a trailing CRLF.
```

