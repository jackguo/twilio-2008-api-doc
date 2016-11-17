<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# Twilio REST Web Service Interface
 
The Twilio REST API allows you to query meta-data about your account, phone numbers, calls, text messages, and recordings. You can also do some fancy things like initiate outbound calls and send text messages.

Since the API is based on REST principles, it's very easy to write and test applications. You can use your browser to access URLs, and you can use pretty much any HTTP client in any programming language to interact with the API.

### Base URL

All URLs referenced in this document have the following base: 

> https://api.twilio.com/2008-08-01 

### HTTP and HTTPS 

The Twilio REST API is served over HTTPS. To ensure data privacy unencrypted HTTP is not supported.

### About REST (REpresentational State Transfer)

We have strived to create a very RESTy interface, so that your consumption of it is simple and straightforward. From [Wikipedia][13]:  

REST's proponents argue that the Web's scalability and growth are a direct result of a few key design principles:  
  
*   Application state and functionality are divided into resources
*   Every resource is uniquely addressable using a universal syntax for use in hypermedia links
*   All resources share a uniform interface for the transfer of state between client and resource, consisting of
*   A constrained set of well-defined operations
*   A constrained set of content types, optionally supporting code on demand
*   A protocol which is:
	*   Client-server
	*   Stateless
	*   Cacheable
	*   Layered
*   REST's client/server separation of concerns simplifies component implementation, reduces the complexity of connector semantics, improves the effectiveness of performance tuning, and increases the scalability of pure server components. Layered system constraints allow intermediaries-proxies, gateways, and firewalls-to be introduced at various points in the communication without changing the interfaces between components, thus allowing them to assist in communication translation or improve performance via large-scale, shared caching. REST enables intermediate processing by constraining messages to be self-descriptive: interaction is stateless between requests, standard methods and media types are used to indicate semantics and exchange information, and responses explicitly indicate cacheability. 

If you're looking for more information about RESTful Web Services, the [O'Reilly Book RESTful Web Services][14] is excellent.

 [13]: http://en.wikipedia.org/wiki/Representational_State_Transfer
 [14]: http://www.amazon.com/RESTful-Web-Services-Leonard-Richardson/dp/0596529260
