
# Web API's 
## Things to consider when designing an API 
- what the API does?
- What the API makes easy?
- what the user wants to do? 
- Document every step you make. 


# REST API 
well-designed web API should aim to support:

**Platform independence**
Any client should be able to call the API, regardless of how the API is implemented internally. this require standard protocols and agreement on the format of the data exhanged. 

**Service Evolution** 
The web API should be able to evolve and add functionality independently from client applications. existence applications can keeo use the api without modification.

All funcionality should be discoverable.

## What is REST 
REST is an architectural style for building distributed systems based on hypermedia. REST is independent of any underlying protocol and is not necessarily tied to HTTP.

A primary advantage of REST over HTTP is that it uses open standards, and does not bindthe implementation of the API or the client applications to any specific implementation.

## Design Princibles of REST API 
- REST APIs are designed around
resources, which are any kind of object, data, or service that can be accessed by the client. 
- A resource has an identifier, which is a URI that uniquely identifies that resourcse. 
- Clients interact with a service by exchanging representations of resources. Many web APIs use JSON as the exchange format. 
- REST APIs use a uniform interface, which helps to decouple the client and service implementations. For REST APIs built on HTTP, the uniform interface includes using standard HTTP verbs to perform operations on resources.  
- REST APIs use a stateless request model. HTTP requests should be independent and may occur in any order, so keeping transient state information between requests is not feasible. The only place where information is stored is in the resources themselves, and each request should be an atomic operation. This constraint enables web services to be highly scalable, because there is no need to retain any affinity between clients and specific servers.
- REST APIs are driven by hypermedia links that are contained in the representation. **HATEOAS** 

## Organize API Around Resources 
- Focus on the business entities that the web API exposes. 
- resource URI should be based on nouns(the reource) not verbs(the action performed). 
- A resource doesn't have to be based on a single physical data item. 
- Entities are often grouped together into collections (orders, customers). A collection is a
separate resource from the item within the collections, and should have its own URI.  
- Adopt a consistent naming convention in URIs, use plural nouns for resources that respresents collection. 
- organize URIs for collections and items into a hierarchy. */customers/5* 
- consider the relationships between different types of resources and how you might expose these associations
    - A better solution is to provide navigable links to associated resources in the body of the HTTP response message. 
- **Avoid requiring resource URIs more complex than collection/item/collection** 
-  avoid "chatty" web APIs that expose a large number of small resources.  denormalize the data and combine related information into bigger resources that can be retrieved with a single request. balance this approach with retriving large objects can increase latency and inccure additional bandwidth. 
- Avoid introducing dependencies between the web API and the underlying data sources. think of the web API as an abstraction of the database. 
- it might not be possible to map every operation implemented by a web API to aspecific resource. You can handle such non-resource scenarios through HTTP requests that invoke a function and return the results as an HTTP response message. *URI/add?perand1=2&operand2=6*


## Define api operations in terms of HTTP methods 

**GET:** retrieves a representation of the resource at the specified URI. The body of the response message contains the details of the requested resource.

**POST:** creates a new resource at the specified URI. The body of the request message provides the details of the new resource. t POST can also be used to trigger operations that don't actually create resources.

**PUT:** either creates or replaces the resource at the specified URI. The body of the request message specifies the resource to be created or updated.

**PATCH** performs a partial update of a resource. The request body specifies the set of changes to apply to the resource.

**DELETE** removes the resource at the specified URI. 


## Difference between POST, Patch, PUT
- ### POST 
request creates a resource. The server assigns a URI for the new resource, and returns that URI to the client. 
POST applied to collection, POST request can also be used to submit data for processing to an existing resource, without any new resource being created.   

Post used for anything else that does't fit the other verbs.

- ### PUT
A PUT request creates a resource or updates an existing resource. If a resource with this URI already exists, it is replaced. Otherwise a new resource is created, if the server supports doing so. 

applied to single item, A server might support updates but not creation via PUT. then use POST to create resources and PUT orPATCH to update. 

PUT requests must be idempotent. If a client submits the same PUT request multiple times, the results should always be the same. 

Consider implementing bulk HTTP PUT operations that can batch updates to multiple resources in a collection
- ### PATCH
performs a partial update to an existing resource, The request body specifies a set of changes to apply to the resource. 

PATCH can also create a new resource (by specifying a set of updates to a "null" resource), if the server supports this. 
POST and PATCH are not guranteed to be idempotent.  

With a PATCH request, the client sends a set of updates to an existing resource, in the form of a patch document. The server processes the patch document to perform the update. The *patch document* doesn't describe the whole resource, only a set of changes to apply.

## Media Types 
clients and servers exchange representations of resources, In the HTTP protocol, formats are specified through the use of
media types, also called MIME types. 

The **Content-Type** header in a request or response specifies the format of therepresentation. 

If the server doesn't support the media type, it should return HTTP status code **415(Unsupported Media Type)**.

A client request can include an Accept header that contains a list of media types the client will accept from the server in the response message. 

If the server cannot match any of the media type(s) listed, it should return HTTP status code **406 (Not Acceptable)**.

## **Asynchronous operations** 
consider making operations async if it takes a while to complete. return a HTTP response (accepted 202) to indicate the request was accepted for proccessing but is not completed. 

You should expose an **endpoint** that returns the status of an asynchronous request, so the client can monitor the status by polling the status endpoint. Include the URI of the status endpoint in the Location header of the 202 response


if GET return the current status of the request 
if POST the status endpoint should return (303 see other) include a location header to created resource. 


**responses with empty should return status code 204 No Content** 


## **Filter and Paginate** 
Exposing a collection of resources through a single URI can lead to applications fetching large amounts of data when only a subset of the information is required. 
It wastes network bandwidth and processing poweron the server hosting the web API.


- the API can allow passing a filter in the query string of the URI, such as /orders?minCost=n.

- You should design a web API to limit the amount of data returned by any single request.  */orders?limit=25&offset=50*
- Also consider imposing an upper limit on the number of items returned, to help prevent Denial of Service attacks. include some form of metadata that indicate the total number of resources available in the collection.

- sort data as it is fetched */orders?sort=ProductId*.
- limit the fields returned for each item. */orders?fields=ProductID,Quantity* 

**Give all **optional** parameters in query strings meaningful defaults.**


## Partial Responses For Large Binary Objects 
consider enabling such resources to be retrieved in chunks. 
web API should support the Accept-Ranges header for GET requests for large resources. This header indicates that the GET operation supports partial requests. 

can submit GET requests that return a subset of a resource, specified as a range of bytes.

*Accept-Ranges: bytes* 
the Accept-Rangesheader indicates that the corresponding GET operation supports partial results. 

you specify the rang of bytes for each GET using  *Range: bytes=0-2499* 



## HATEOAS 
One of the primary motivations behind REST is that it should be possible to navigate the entire set of resources without requiring prior knowledge of the URI scheme


Each HTTP GET request should return the information necessary to find the resources related directly to the requested object through hyperlinks included in the response, and it should also be provided with information that describes the operations available on each of these resources. The system is effectively a finite state machine, and the response to each request contains the information necessary to move from one state to another; no other information should be necessary


## API Versioning 
Versioning enables a web API to indicate the features and resources that it exposes, and a client application can submit requests that are directed to a specific version of a feature or resource.

The primary imperative is to enable existing client applications to continue functioning unchanged while allowing new client applications to take advantage of new features AND resources. 
 

### Versioning Approaches
- ### No Versioning: 
    - acceptable for some internal APIs.  
    - 
- ### URI Versioning
    - Each time you modify the web API or change the schema of resources, you add a version number to the URI for each resource.
    - This versioning mechanism is very simple but depends on the server routing the request to the appropriate endpoint. 

- ### Query String Versioning 
    - you can specify the version of the resource by using a parameter within the query string appended to the HTTP request, such as *https://adventure-works.com/customers/3?version=2* 
- ### Header Versioning
    - implement a custom header that indicates the version of the resource. *Custom-Header: api-version=1* 
- ### Media Type Versioning
    - define custom media types that include information enabling the client application to indicate which version of a resource it is expecting. *Accept: application/vnd.adventure-works.v1+json*
