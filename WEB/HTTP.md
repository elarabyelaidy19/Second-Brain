# HTTP 

## Messages 
### Request and response 
HTTP is a request-and-response protocol. A client sends an HTTP
request to a server using a carefully formatted message that the
server will understand. A server responds by sending an HTTP
response that the client will understand. The request and the
response are two different message types that are exchanged in a
single HTTP transaction. 

The HTTP standards define what goes into
the request and response messages so that everyone who speaks
“HTTP” will understand each other and can exchange resources.
When a resource does not exist, a server can send not found response. 

a web browser know how to send an http request by opining a network connection to a server machine and sending an http message as text. 

The message is a command in plain ASCII text and forma􀆩ed per the
HTTP specification. 

sometimes you need to include the host name in the message, because there is could be multiples sites in the same server. the host info will help the web server to direct the request to right site. 

the redirect ensures all the requests for a resource from current to go through it to the redirected destination.


### HTTP Request Methods: 
Every request message must include an HTTP
method. The method describes the purpose of the request. The
purpose of an HTTP GET request is to read, fetch, or retrieve a
resource. 

| Method | Description                                |
|--------|--------------------------------------------|
| GET    | Retrieve a resource                         |
| PUT    | Store a resource                            |
| PATCH  | Partial update of a resource                |
| DELETE | Remove a resource                           |
| POST   | Update a resource                           |
| HEADER | Retrieve the headers or metadata of a resource |


### Safe Methods 
The **GET method** is one of the safe methods. A GET should
only retrieve a resource and not alter the state of the resource. there is no side effects of a GET Method. 

An **HTTP POST** is not a safe method. A POST typically changes
something on the server.

The form inputs are included in in message body, it's like query string appearing in URL.

**responding to http post request**
- Respond with HTML to show the successful account creation.
- Respond with a redirect message, and force the browser to issue a safe GET request for a page that tells him about
the successful account creation. 
- Respond with an error, or redirect to an error page. 

### Forms and get request: 
- making a search using form issues an HTTP get request using the query string in the URL.
- there is can be a virtual resource /account/create is a virtual resource.

### HTTP Headers: 
- header contain useful information that can help server process the request. it can be useful in such content negotiation.
- client or a server can include a Date header indicate a resource creation date.
**Some http headers**
- **Referrer** When the user clicks on a link, the client can send the URL of the referring page in this header.
- **User Agent**: information about the user agent/client that makes the request.
- **Accept**: the media the user agent will accept. 
- **Accept-Language** Describes the languages the user-agent prefers.
- **Cookie**: info's helps server identify user.


### The Response status code: 
An HTTP response has a similar structure to an HTTP request. 
- **status code** tells client what happened to the request.
- A response includes header info that helps the client process the response. 
- The headers can also contain info about the server, and the technology behinds the website.


## Connections 

Network communications consist of layers each layer responsible for a specific and limited number of responsibility. 

HTTP is an application layer protocol that allow two applications to communicate over the network. quite often one is the browser and the other is a web server. 

The transmission of an HTTP message is the job of a lower layer protocol. A message from a web browser has to travel down a series of network layers.

### Request/Response lifecycle

The layer underneath HTTP is a transport layer protocol.
Almost all HTTP traffic travels over TCP (short for Transmission
Control Protocol), although this is not required by HTTP. 


- When a user types a URL into the browser, the browser first extracts the host name from the URL (and port number, if any), and opens a TCP socket by specifying the server address (derived
from the host name) and port (which defaults to 80).

- once application have open socket it can write data to the socket
- the TCP layer accepets the data and gurantees the delivry of a message.
- TCP is a reliable protocol which will resend any data lost in transmit. 

- TCP also have flow control mechanism which ensure data not transmit too fast for the reciver to process or too slow. 

### Internet Protocol / IP 
IP is responsible for taking information and moving them through networks devices from one network to another. it does not gurantee delivery of message. 

- every device have and address, breaking data into packets, fragmentin and reassembling thes packets.


### Data Link Layer 
but eventually these IP packets must travel over a
piece of wire, a fiber ooptic cable, this is the resposibility of data link layer/ Ethernet. 


the signal reaches the server and enters the
machine through a network card where the same process
happens in reverse.


TCP understands how to
manage multiple connection to the same server so you can have two concurrent requests to the same server using two differnt sockets. 

### WireShark 

Wireshark is
a network analyzer that can show you every bit of information flowing through your network interfaces. 


- HTTP relies almost entirely on TCP to take care of all the hard work and tcp involve some over head such handshacked. 


### HTTP Performance

Internt is full of latency, signals must travel long distance, there is also some overhead in the underlying TCP connection.
there is a 3-step handshake to complete before an HTTP transaction 

#### **Parallel connection** 
opening multible parallel connections to a server. if there is multiple resources on a page the browser will open as many as resources connections in parallel to download. 

Parallel connection will obey the law of diminishing returns, too many connections can saturate and congest the network, in addition web server can only accepts a finite number of connections. 

#### **Persistent Connection**
in the very old days browsers will open and close a tcp connection for each individual HTTP Transaction. this implementaion was per HTTP's is **Stateless** every request/responce does't know anything about each other, infos web server needs will be sent every request header. 

to solve this problem C/S should impelement persistent connection and make it the default. it's stays open after each http transaction completion that can be used in future request without the overhead of opening a new socket. 


persistent connection avoids the slow start strategy which avoid sending more data than a network can handle by starting in slow rate and increase gradually. 

Persistent connection reduce memory usage, cpu, network congestion, latency. 




Many servers have a default configuration to limits the number of concurrent connections far below the point where the server will fail. it's a security mesure help prevents ths DoS attacks.  whcih create multible tcp persistent tcp connections overwhelming the server which inhibit the server from responding to real customers. 


However, because a server only supports a finite number of connections web server will colose the connection if it's idle for some period. additionlay you can disabel persistent connection as shared servers does.

#### **Pipelined Connections** 
you can sent multiple request on the same connection before waiting to the first response. 


## Web Archeticture


### Resources Redux
a resource is can be think of as being a filr on a server's file system. 

The resource could be a document on the file system, but a web framework could also respond to the request for the resource and build the resource using information stored in files stored in database, retrived from web services. 

A URL can not specify the representaation of a resource, and a resource can have multipel representations. 
A URL Can not say what a specific user want to do with a respurce. it's the job of HTTP request Method. 

HTTP offers these benefits because a URL is just a pointer, or a unit of indirection between a client and application server. 


All the information required for and HTTP Transaction is contained in the request/response messages. and it's visible and easy to parse. 


### Adding Value 
HTTP message moves from the memory space in one process one machine to memory space in one process on another machine. 

a web server like apache will be one of the first recipent of an HTTP request on a server machine. 
the web server can inspect the info from the message, and can do additional actions with message like logging to a local file. and the same can be happend in the response. like compression, loggine, adding info to the message etc.. 

### Proxies 
A proxy server is computer sits between the client and the server. a proxy is mostly transparent to the the users, once you send an request the request goes to a proxy sever and the proxy will forwards the request to the desired serve. and then take the response and forwards it back to the client. it can inspect the messahe and take additional actions. 


you can blocks suers from accesing some sebsites by capture all http traffic and deny those requests to reach their distination. 
can inspect message to remove confidential data, **Access Control Proxy** can log HTTP messages to create audit trails. can controle some user from accessing external network resources. 

- **Forwoard Proxy:** **ACP** is of forwards proxy, forward proxy is closer to a client than a server. 
- **Reverse Proxy** Closer to server than the client 

- a proxy server has the capability to compress response message bodies. A company might use a reverse proxy server for compression to take the computational load pff the web server where the application lives. 
- compression is a feature that layers into an architecture via a proxy server

### Popular Proxy Servers

#### **Load Balancer**  
Can take a message and forwards it to a web server based on a round robin basis, or which server currently proccessing the least number of requests. 
#### **SSL Acceleration**
- Can encrypt and decrypt a message taking the encryption load oof a webs server. 
- Can provide an additional layer of security by filtiring out potentially dangerous messages specifically, XSS, Sql injection. 


#### **Caching Proxies**
Caching proxies will store copies of frequently accessed
resources. The proxy can respond to messages requested  by returning the cached ones directly.

Proxies are a perfect example of how HTTP can influence the
architecture of a web application. 


### **Caching**
Caching is an optimization made to improve performance and scalabilty, when there is a resource that accessed frequently we can use Proxy to cach this resource and reduce time and bandwidth required for a full retival. and reduce latency.

#### Types Of Cache

- **Public Cache** 
    -   is shared amongs multiple users and it's popular in acommunity of users if it saved on Forward Proxy.
    - cache on a reverse proxy the reources that are poular on a specific websites. 

- **Private Cache** 
    - A private cache is a cache for a single user. Web browsers
always keep a private cache of resources on your disk.  

An HTTP response can have a value for **Cache-control** of
public , private or no-cache .  
- **public** :proxy servers can cache the response. 
- **private**:means only the user’s browser can cache the response. 
- **no-cache** :means no one should cache the response.
- **no-store value**: meaning the message might
contain sensitive information and should not be cached and deleted from brqowswer memory as soon as possible.
- **max-age** value in the Cachecontrolheader. The max-age value is the number ofseconds to cache the response.
- **Last-Modified** header to indicate when the resource representation last changed. 


## State And Security 
HTTP is a stateless protocol, each request/response transaction is independent of any previous or future transaction. 
Every request will carry all the information a serever needs to create the response.

### Two approaches to save state 
- embed state in the resource that transfared to the client, state required by application will travel back on the next request. 
    - lasts for short-lived period
    - examples three pages application process
- store the state on  the server (session based state)
    - lasts for long-time 
     
## Cookies
Cookies is `State Managment Mechanizm` that used for tracking and differentiate one user from another, by setting a unique idintifier in the cokkie which is transfered with each R/R headers. 

Cookies Size is 4KB

when disable cookie you can not track what the user do on yout website and can take  another approach Cookieless session
**cookieless sessions** is to place the user identifier into the URL. Cookieless sessions require that every URL a site gives to a
user must contain the proper identifir nd the URLs become much larger (which is why we call this technique the “fat URL”
technique).

**Setting Cookies**  
you can put what you want in the cookie but usually it put a unique identifier GUID, and keet the session data on the server.   

```shell 
set-cookie = fname=Scott&lname=Allen;
domain=.mywebsite.com; path=/
```
cookies will be sent in each subsequent request, when the ID arrive to the server it will lookup for any associated user data from in-memory data structure, database, cache. 


data stored in the session object is not living in the cookie. The cookie only contains a session identifier, the values associated with session identifier is stored on the server.  

Web application compact the problem of guessing session id by generating a larger random id.

### HTTP-ONLY Cookies 
it's used to prevent XSS(Cross Site Attacks) a malicious person inject malevolent JavaScript into someone website that can manipulate and steal cookie when user runs this script. it's prevent JS from access cookies.

### Types Of Cookies
- Session-Cookies: it last for a single-user session and destroyed when leave the site or close the browser
- Persistent-Cookies: can live for long time and stored on the disk. 

the differences in the `expires` flag. 

### Cookies Path && Domain
The only cookies a browser should send to a site are the cookies given to the browser by the same site. 

The web application controls the scope using domain and path attributes.  

The path attribute can restrict a cookie to a specific resource
path. In the above example, the cookie will only travel to a
server.com site when the request URL is pointing to /stuff, or any path underneath.  

### Cookie Downside 
- vulnerable for XSS Attacks. 
- bad publicity when using third-parties cookies when another set set scripts on website you visit and set-cookie when you run those scripts. 

```json 
domain=.server.com;
path=/stuff
```



