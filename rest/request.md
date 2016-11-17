<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST Request Format

### Credentials 

All requests to Twilio's REST API require you to authenticate. Use HTTP basic auth to convey your identity to the API when making requests. Your username is your account sid (a 34 character string, starting with the letters AC). Your password is your auth token. You can find both your account sid and auth token on your [Account Dashboard][1] page.

Most HTTP clients (including web-browsers) have a way of providing a username and password for HTTP basic auth. Most clients will also allow you to provide them in the URL itself. For example:

> https://username:password@api.twilio.com/2008-08-01/...

### Retrieving Resources with the HTTP GET Method

You can retrieve a representation of a resource by GETting its url. The easiest way to do this is to copy and paste a URL into your web browser's address bar.

#### Possible GET Response Status Codes:

*   **200 OK**: The request was successful and the response body contains the representation requested.
*   **302 FOUND**: A common redirect response; you can GET the representation at the URI in the Location response header.
*   **304 NOT MODIFIED**: Your client's cached version of the representation is still up to date.
*   **401 UNAUTHORIZED**: The supplied credentials, if any, are not sufficient to access the resource.
*   **404 NOT FOUND**: You know this one.
*   **500 SERVER ERROR**: We screwed up and couldn't return the representation.

### Creating or Updating Resources with the HTTP POST and PUT Methods

Creating or updating a resource involves performing an HTTP PUT or HTTP POST to a resource URL. In the PUT or POST, you represent the properties of the object you wish to update as form urlencoded key/value pairs. Don't worry, this is already the way browsers encode POSTs by default.

#### Possible POST or PUT Response Status Codes:

*   **200 OK**: The request was successful, we updated the resource and the response body contains the representation.
*   **400 BAD REQUEST**: The data given in the POST or PUT failed validation. Inspect the response body for details.
*   **401 UNAUTHORIZED**: The supplied credentials, if any, are not sufficient to create or update the resource.
*   **404 NOT FOUND**: You know this one.
*   **405 METHOD NOT ALLOWED**: You can't POST or PUT to the resource.
*   **500 SERVER ERROR**: We screwed up and couldn't create or update the resource.

### Deleting Resources with the HTTP DELETE Method

To delete a resource make an HTTP DELETE request to the resource's URL. Not all Twilio REST API resources support DELETE.

#### Possible DELETE Response Status Codes:

*   **200 OK**: The request was successful; the resource was deleted.
*   **401 UNAUTHORIZED**: The supplied credentials, if any, are not sufficient to delete the resource.
*   **404 NOT FOUND**: You know this one.
*   **405 METHOD NOT ALLOWED**: You can't DELETE the resource.
*   **500 SERVER ERROR**: We screwed up and couldn't delete the resource.

### HTTP Method Overloading

Twilio's REST API uses HTTP GET, POST, PUT and DELETE methods. Since some HTTP clients do not support methods PUT and DELETE, you can simulate them via POST by appending the query string parameter _method (yes, underscore method) to a resource URL. Valid values are PUT and DELETE.

For example, if you want to perform a DELETE request on a particular phone number resource you could:
> DELETE /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/PhoneNumbers/PN12345567789AFE4433 HTTP/1.1

But if your client is only capable of GET and POST, then you could perform a POST with a _method query string variable to achieve the same result:
> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/PhoneNumbers/PN12345567789AFE4433?_method=DELETE HTTP/1.1

[1]: /user/account/
