<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: Twilio Responses

### Response Formats

#### Resource Representations

By default Twilio's REST API returns resource representations as XML within a root element of `<TwilioResponse>`. For example, the default representation of an account resource is:  

> GET /2008-08-01/Accounts/AC35542fc30a091bed0c1ed511e1d9935d HTTP/1.1

~~~
<TwilioResponse> 
	<Account> 
		<Sid>AC35542fc30a091bed0c1ed511e1d9935d</Sid> 
		<FriendlyName>Friendly Account 1</FriendlyName> 
		<Status>1</Status> 
		<StatusText>Inactive</StatusText> 
		<DateCreated>Tue, 01 Apr 2008 11:26:32 -0700</DateCreated> 
		<DateUpdated>Tue, 01 Apr 2008 11:26:32 -0700</DateUpdated> 
	</Account>
</TwilioResponse>
~~~

Twilio can also return JSON, CSV and other representation formats. See the [Tips & Tricks][tips] section for more information.

[tips]: tips

#### Exceptions 

Twilio returns exceptions as a `<RestException>` element within the root `<TwilioResponse>` element. The RestException contains up to four elements:

*   **Status**: Corresponds to the HTTP status code returned.
*   **Message**: A more verbose message regarding the error.
*   **Code**: (Conditional) An error code that can be used to find specific help in the Twilio documentation.
*   **MoreInfo**: (Conditional) The URL of Twilio's documentation for the error code.

If you receive a 400 response status (invalid request), then the `<Code>` and `<MoreInfo>` elements will be populated to help you debug your invalid request.

For example, a simple 404: 

~~~
<TwilioResponse> 
	<RestException> 
		<Status>404</Status> 
		<Message>The requested resource was not found</Message> 
	</RestException> 
</TwilioResponse>
~~~

Another example; an invalid POST to the Calls resource that was missing the Called parameter: 

~~~
<TwilioResponse> 
	<RestException> 
		<Status>400</Status> 
		<Message>No called number is specified</Message> 
		<Code>21201</Code>
		<MoreInfo>http://www.twilio.com/docs/errors/21201</MoreInfo>
	</RestException> 
</TwilioResponse>
~~~

#### List Resources 

Some resources are lists of other resources. For example, the Calls resource returns a list of calls. There are several things to know about using and manipulating these lists:

##### Paging Info {#paging}

If lists are long, the API returns a single page of results. The XML response returns paging info in the list's element, including:

Parameter     |   Description
--------------|--------------
page | which page you're currently viewing. Zero-indexed, so the first page is 0.
numpages | How many pages of data are there in total
pagesize | How many objects are on each page
total | How many total objects are there in the result set
start | The index of the first object on this page, out of the total result set size.
end | The index of the last object on this page, out of the total result set size.

In addition to this information, the API returns URIs to the next, previous,
first and last pages of the returned list, making navigating paged results a
breeze:

Parameter     |   Description
--------------|--------------
uri    |    The URI of the current page.
firstpageuri    |    The URI for the first page of this list.
nextpageuri    |    The URI for the next page of this list.
previouspageuri    |    The URI for the previous page of this list.
lastpageuri    |    The URI for the last page of this list.


For example:  
  
~~~
<TwilioResponse> 
	<Calls page="0" numpages="1" pagesize="50" total="38" start="0" end="37"> 
		<Call> 
			<Sid>CA42ed11f93dc08b952027ffbc406d0868</Sid> 
			<CallSegmentSid/> 
			<AccountSid>AC012345678901234567890123456789AF</AccountSid> 
			<Called>4159633717</Called> 
			<Caller>4156767925</Caller> 
			<PhoneNumberSid>PN01234567890123456789012345678900</PhoneNumberSid> 
			<Status>2</Status> 
			<StartTime>Thu, 03 Apr 2008 04:36:33 -0400</StartTime> 
			<EndTime>Thu, 03 Apr 2008 04:36:47 -0400</EndTime> 
			<Price/> 
			<Flags>1</Flags> 
		</Call> 
		... 
	</Calls> 
</TwilioResponse>
~~~

When fetching multiple pages of API results, use the provided `nextpageuri`
parameter to retrieve the next page of results. All of the [Twilio Helper
Libraries](/docs/libraries) use the `nextpageuri` to page through resources.
  
You can control the size of pages with the `num` parameter:

Parameter     |   Description
--------------|--------------
num | How many resources to return in each list page. The default is 50, and the maximum is 1000.

For example, to limit the number of calls returned to five per page:

> GET /2008-08-01/Accounts/{AccountSid}/Calls?num=5

<div class="alert alert-error">
The <code class="notranslate">Page</code> parameter has been deprecated and may be removed in a
future version of the API. The <code class="notranslate">Page</code> parameter is slower than the
<code class="notranslate">nextpageuri</code>, and if new resources are created while paging with
the <code class="notranslate">Page</code> parameter, consecutive pages may contain duplicate data.
</div>

### HTTP Status Codes
 
Twilio frequently returns the following status codes:
  
*   **200 OK:** All is well, the request body is a representation of the requested resource.
*   **201 CREATED:** Your request resulted in a new resource being created. Its URL is given in the http Location header.
*   **302 FOUND:** A common redirect response, look for the new resource URI in the Location header.
*   **304 NOT MODIFIED:** Means that the client's cached version is still up to date.
*   **400 BAD REQUEST:** Something in the request was not valid, usually inputs to a POST or PUT. Check the RestException/Message string for more details.
*   **401 UNAUTHORIZED:** The supplied credentials, if any, are not sufficient to access the resource.
*   **404 NOT FOUND:** You know this one.
*   **405 METHOD NOT ALLOWED:** The HTTP method (GET, POST, PUT, DELETE) is not valid for the resource. For example, trying to DELETE your account. Bad idea.
*   **500 SERVER ERROR:** We screwed up. Sorry.
