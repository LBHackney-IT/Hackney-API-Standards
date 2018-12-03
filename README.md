# Hackney-API-Standards
Hackney API Standards

![APIFirstStrategy](images/APIFirstStrategy.png)

### 1.1.1      RESTful is the key
As a developer I should always build pragmatic API designed for today's web applications.

Some requirements that the API must strive for:
We should use web standards
Someone without prior knowledge, or someone to ask must be able to understand how to consume the API
It should be simple, intuitive and consistent to make adoption as pleasant experience as possible.
It should provide enough flexibility.
It should be efficient, while maintaining balance with the other requirements.

### 1.1.2      Use RESTful URLs and actions

As a developer I should ensure that I separate API into logicat resources so that method has specific meaning.

The key principles of REST involve separating API into logical resources. These resources are manipulated using HTTP requests where the method (GET, POST, PUT, PATCH, DELETE) has specific meaning.

### 1.1.3      SSL everywhere - all the time

As a developer I should ensure that I use SSL all the time so that authentication for accessing API are secured.

We must always use SSL. Web APIs will be accessed from anywhere wherever internet is available. There are case where few connections made are not secure enough. Many don't encrypt communications at all, allowing for easy eavesdropping or impersonation if authentication credentials are hijacked.
 
Another advantage of always using SSL is that guaranteed encrypted communications simplifies authentication efforts - you can get away with simple access tokens instead of having to sign each API request.
 
One thing to watch out for is non-SSL access to API URLs. Do not redirect these to their SSL counterparts. Throw a hard error instead! The last thing we want is for poorly configured clients to send requests to an unencrypted endpoint, just to be silently redirected to the actual encrypted endpoint

### 1.1.4      Documentation
As a developer I should ensure that API has good documentation attached so that wider audience can easily understand it.

An API is only as good as its documentation. The documentations should be easy to find and publically accessible. The docs should show examples of complete request/response cycles. It is recommended that the requests should be pastable examples - either links that can be pasted into a browser or curl examples that can be pasted into a terminal.
.
### 1.1.5      Versioning
As a developer I should version API so that it allows smooth transitions over the time and it is very helpful in order to make sure it is not break any existing integrations.
Developers should always version their API. Versioning helps to iterate faster and prevents invalid requests from hitting updated endpoints. It also helps smooth over any major API version transitions as you can continue to offer old API versions for a period of time.
 
### 1.1.6      Result filtering, paging, sorting & searching
As a developer I should keep the base resource URLs as lean as possible so that all be easily implemented as query parameters on top of the base URL.
It's best to keep the base resource URLs as lean as possible. Complex result filters, paging, sorting requirements and advanced searching (when restricted to a single type of resource) can all be easily implemented as query parameters on top of the base URL. 

- Filtering: Use a unique query parameter for each field that implements filtering. For example, when requesting a list of tickets from the /tickets endpoint, you may want to limit these to only those in the open state. This could be accomplished with a request like GET /tickets?state=open. Here, state is a query parameter that implements a filter.

- Paging: Large data sets should be paged when being returned.  This will reduce the effort when returning data to a client.  The client should be able to specify the start index and page count so that the api can know from which record to start and how many records should be returned.
For example:
- GET /tickets/page_start=0&page_count=50 would return the first 50 records.
- GET /tickets/page_start=70&page_count=10 would return the next 10 records after the 70th record.
The last paging info should be returned as part of the response.  The client will know that the api is at the end of the dataset if the number of items returned is less than the number requested.

- Sorting: Similar to filtering, a generic parameter sort can be used to describe sorting rules. Accommodate complex sorting requirements by letting the sort parameter take in list of comma separated fields, each with a possible unary negative to imply descending sort order. Let's look at some examples:
  - GET /tickets?sort=-priority - Retrieves a list of tickets in descending order of priority
  - GET /tickets?sort=-priority,created_at - Retrieves a list of tickets in descending order of priority. Within a specific priority, older tickets are ordered first.

- Searching: When full text search is used as a mechanism of retrieving resource instances for a specific type of resource, it can be exposed on the API as a query parameter on the resource's endpoint. Let's say q. Search queries should be passed straight to the search engine and API output should be in the same format as a normal list result.
Combining these together, we can build queries like:
  - GET /tickets?sort=-updated_at - Retrieve recently updated tickets
  - GET /tickets?state=closed&sort=-updated_at - Retrieve recently closed tickets
  - GET /tickets?q=return&state=open&sort=-priority,created_at - Retrieve the highest priority open tickets mentioning the word 'return'

- Aliases for common queries
To make the API experience more pleasant for the average consumer, consider packaging up sets of conditions into easily accessible RESTful paths. For example, the recently closed tickets query above could be packaged up as GET /tickets/recently_closed

### 1.1.7      HTTP Status codes
As a developer I should return valid HTTP status codes when request is failed,passed or the request was wrong in a meaningful way.

When the client raises a request to the server through an API, the client should know the feedback, whether it failed, passed or the request was wrong. HTTP status codes are bunch of standardized codes which has various explanations in various scenarios. The server should always return the right status code.

- 2xx (Success category)

These status codes represent that the requested action was received and successfully processed by the server.
  - 200 Ok The standard HTTP response representing success for GET, PUT or POST.
  - 201 Created This status code should be returned whenever the new instance is created. E.g on creating a new instance, using POST method, should always return 201 status code.
  - 204 No Content represents the request is successfully processed, but has not returned any content.

- 3xx (Redirection Category)
  - 304 Not Modified indicates that the client has the response already in its cache. And hence there is no need to transfer the same data again.

- 4xx (Client Error Category)
These status codes represent that the client has raised a faulty request.
  - 400 Bad Request indicates that the request by the client was not processed, as the server could not understand what the client is asking for.
  - 401 Unauthorized indicates that the client is not allowed to access resources, and should re-request with the required credentials.
  - 403 Forbidden indicates that the request is valid and the client is authenticated, but the client is not allowed access the page or resource for any reason. E.g sometimes the authorized client is not allowed to access the directory on the server.
  - 404 Not Found indicates that the requested resource is not available now.
  - 410 Gone indicates that the requested resource is no longer available which has been intentionally moved.

- 5xx (Server Error Category)
  - 500 Internal Server Error indicates that the request is valid, but the server is totally confused and the server is asked to serve some unexpected condition.
  - 503 Service Unavailable indicates that the server is down or unavailable to receive and process the request. Mostly if the server is undergoing maintenance.

### 1.1.8 JSON responses
As a developer I should return API response as JSON so that it is easy to read and parsed easily.
JSON response are easy to parse,easy to read. This gives an advantage over XML. The lightweight approach of JSON can make significant improvements in RESTful APIs working with complex systems. Also JSON uses a map data structure rather than XML's tree. In some situations, key/value pairs can limit what you can do, but you get a predictable and easy-to-understand data mode

### 1.1.9 Containerisation

As a developer I should make sure I make my application as containerised as possible.

Containerization is an Operating System level virtualization method for deploying and running distributed applications without launching Virtual Machines(VMs) for each application.This approach is particularly useful in development.Usually when you start a new job you would get a step-by-step process to follow to configure your machine or a virtual machine image with a pre-configured environment. However, with Docker containers it is all about installing Docker and pulling/running containers to kick off the development environment.  

### 1.1.10 Health Checks

As a developer I should make sure that I build ‘Health Check API endpoints’ to monitor is service up and report back if it goes down.

Also health checks can also be used by monitoring tools to track and alert on the availability and performance of the service, where they serve as early problem indicators. For eg Sentry for exception logging,New relic for application monitoring.


### 1.1.11 Error Handling

As a developer I should make sure that the api handles errors in a consistent way.

It is strongly recommended that you have a global error handler. This removes the need to have verbose error handling inside your codebase. 

### 1.1.12 TDD (Test Driven Development)

As a developer I should ensure that I write well defined tests around all functionalities so that it ensures we write better and cleaner code.

Test-driven development (TDD) is a well-defined approach to create softwares which are robust and maintainable. It helps you to write better and cleaner code. Also by having solid tests will give you a new sense of confidence when you need to modify an existing code.

### 1.1.13 Centralised Application Monitoring

As a developer I must ensure that I have application monitoring mechanism in place within the microservice.

We use New Relic as a centralised application performance monitoring tool, as it is capable of instrumenting applications in many languages, including C# and Ruby. It allows us to see requests going through an application and where time is spent during those requests. For example, if a large SQL call is what is hurting performance. These are a useful part of what we call a "layered monitoring approach": start with high-level API monitoring, and use these other tools (exception trackers, APM, raw logs) to dive into issues when they arise

In a .NET application, you install a New Relic Agent on the machine, which will automatically instrument any .NET apps running. You can find an example of setting this up in the Tenancy API.
In a Ruby application, you install the New Relic gem and configure it with environment variables. You can find an example of setting this up in the Income API.

### 1.1.14 Centralised Uptime Monitoring

As a developer I will ensure that API monitoring tool is in place so that if the API can’t  be reached from a location I should be able receive notifications via email alerts etc.

We use Pingdom as a centralised uptime monitoring service, as it's a more cost effective solution than competitors for basic service.
We configure it to make a simple HTTPS request to an endpoint in each application in each environment, every minute, to track uptime of our services.
If the applications go down, automated alerts are sent to responsible team members. This lets them know when they need to take action, and informs them of potential problems in their production environments before users have to raise issues.

### 1.1.15 Centralised exception Monitoring

As a developer I should ensure that centralised exception logging mechanism is in place.

We use Sentry as a centralised exception logging platform, as it's able to receive exceptions from many languages, including C# and Ruby, and has good tooling around releases. It logs every error which occurs in any of our systems, attaching metadata including which user experienced the error, what request they were trying to make, how many times that error has occurred, and which code release likely introduced the error.

It is very useful for developers/maintainers to investigate and fix application errors, including more information to aid diagnosis than stack traces in logging will.
In a .NET application, you install a Sentry package using NuGet and use it to send messages to Sentry when handling uncaught exceptions. You can find an example of this in the Tenancy API.
In a Ruby application, you install the Sentry gem and configure it to add additional context. You can find an example of setting this up in the Income API.


