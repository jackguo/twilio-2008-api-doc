<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: Notifications

The Notifications list resource represents a list of notifications. A Notification instance resource represents a single log entry made by Twilio in the course of handling your calls or your use of the REST API. It is very useful for debugging purposes.



### Notification Instance Resource 

This resource represents an individual notification entry.

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/Notifications/{NotificationSid} 

#### Resource Properties 

A Notification resource is represented by the following properties: 

"Notification Resource Properties"
Property      | Description
:------------- | :--------
Sid  | A 34 character string that uniquely identifies this resource.
DateCreated   | The date that this resource was created, given in [RFC 2822][1] format.
DateUpdated   | The date that this resource was last updated, given in [RFC 2822][1] format.
AccountSid    | The 34 character id of the [Account][2] this notification is associated with. (Your account!)
CallSid       | CallSid is the unique id of the call during which the notification was generated. Empty if the notification was generated by the REST API without regard to a specific phone call.
Log           | An integer log level corresponding to the type of notification: ,  0: ERROR, 1: WARNING
ErrorCode     | A unique error code for the error condition. You can lookup errors, with possible causes and solutions, in our [Error Dictionary][3].
MoreInfo      | A URL for more information about the error condition. The URL is a page in our [Error Dictionary][3].
MessageText   | The text of the notification.
MessageDate   | The date the notification was actually generated, given in [RFC 2822][1] format. Due to buffering, this may be slightly different than the DateCreated date.
RequestURL    | The URL of the resource that generated the notification. <br>**If the notification was generated during a phone call:** <br>This is the URL of the resource on YOUR SERVER that caused the notification. <br>**If the notification was generated by your use of the REST API:**<br> This is the URL of the REST resource you were attempting to request on Twilio's servers.
RequestMethod | The HTTP method in use for the request that generated the notification. <br>**If the notification was generated during a phone call:** <br>The HTTP Method use to request the resource on your server. Either GET or POST. <br>**If the notification was generated by your use of the REST API:** <br>This is the HTTP method used in your request to the REST resource on Twilio's servers. One of GET, POST, PUT or DELETE.

#### HTTP Methods

##### GET

<div class="alert alert-error">
SPECIAL NOTE: Unlike other areas of this REST API, the XML representation of a Notification instance is different from that of the Notification elements within the list resource representation. Due to the potentially voluminous amount of data in a notification, the full HTTP Request and Response data is only returned in the Notification instance resource representation.
</div>

"Additional Notification Instance Properties"
 Property         | Description
 ---------------- | --------------------------------------
 RequestVariables | The Twilio-generated HTTP GET or POST variables sent to your server. Alternatively, if the notification was generated by the REST API, this field will include any HTTP POST or PUT variables you sent to the REST API.
 ResponseHeaders  | The HTTP headers returned by your server.
 ResponseBody     | The HTTP body returned by your server.

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Notifications/NO1fb7086ceb85caed2265f17d7bf7981c HTTP/1.1 

~~~    
<TwilioResponse> 
	<Notification> 
		<Sid>NO1fb7086ceb85caed2265f17d7bf7981c</Sid> 
		<DateCreated>Sat, 07 Feb 2009 13:15:19 -0800</DateCreated>
		<DateUpdated>Sat, 07 Feb 2009 13:15:19 -0800</DateUpdated>
		<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid> 
		<CallSid>CA42ed11f93dc08b952027ffbc406d0868</CallSid> 
		<Log>0</Log>
		<ErrorCode>12345</ErrorCode>
		<MoreInfo>http://www.twilio.com/docs/errors/12345</MoreInfo>
		<MessageText>Unable to parse XML response</MessageText>
		<MessageDate>Thu, 03 Apr 2008 04:36:32 -0400</MessageDate>
		<RequestURL>http://yourserver.com/handleCall.php</RequestURL>
		<RequestMethod>POST</RequestMethod>
		<RequestVariables>Caller=4158675309&Called=4155551212...</RequestVariables>
		<ResponseHeaders>Content-Length: 500</ResponseHeaders>
		<ResponseBody>&lt;h2&gt;Error parsing PHP script&lt;/h2&gt;</ResponseBody>
	</Notification> 
</TwilioResponse>
~~~

##### PUT 

Not Supported. 

##### POST 

Not Supported. 

##### DELETE 

Delete this notification from your log. If successful, returns HTTP status 204 (No Content) with no body. 

> DELETE /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Notifications/NO41331862605f3d662488fdafda2e175f HTTP/1.1



### Notifications List Resource

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/Notifications 

#### HTTP Methods

##### GET 

Returns a list of `<Notification>` elements representing notifications generated for your account, under a `<Notifications>` list element that 
	includes [paging information][4]. The list is sorted by DateUpdated, with newest notifications first. Example: 

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Notifications HTTP/1.1 

~~~
<TwilioResponse> 
	<Notifications page="0" numpages="10" pagesize="50" total="498" start="0" end="49"> 
		<Notification> 
			<Sid>NO1fb7086ceb85caed2265f17d7bf7981c</Sid> 
			<DateCreated>Sat, 07 Feb 2009 13:15:19 -0800</DateCreated>
			<DateUpdated>Sat, 07 Feb 2009 13:15:19 -0800</DateUpdated>
			<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid> 
			<CallSid>CA42ed11f93dc08b952027ffbc406d0868</CallSid> 
			<Log>0</Log> 
			<ErrorCode>12345</ErrorCode>
			<MoreInfo>http://www.twilio.com/docs/errors/12345</MoreInfo>
			<RequestURL>http://yourserver.com/handleCall.php</RequestURL>
			<RequestMethod>POST</RequestMethod>
			<MessageText>Unable to parse XML response</MessageText>
			<MessageDate>Thu, 03 Apr 2008 04:36:32 -0400</MessageDate>
		</Notification> 
		<Notification> 
			<Sid>NOe936fdac57d238e56fd346b89820d342</Sid> 
			<DateCreated>Sat, 07 Feb 2009 13:15:19 -0800</DateCreated>
			<DateUpdated>Sat, 07 Feb 2009 13:15:19 -0800</DateUpdated>
			<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid> 
			<CallSid></CallSid> 
			<Log>1</Log> 
			<ErrorCode>67890</ErrorCode>
			<MoreInfo>http://www.twilio.com/docs/errors/67890</MoreInfo>
			<RequestURL>
			    https://api.twilio.com/2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls
			</RequestURL>
			<RequestMethod>POST</RequestMethod>
			<MessageText>
			    Unknown parameter received by the REST API: foo=bar
			</MessageText> 
			<MessageDate>Thu, 03 Apr 2008 04:36:32 -0400</MessageDate> 
		</Notification> 
		... 
	</Notifications> 
</TwilioResponse>	
~~~

##### URL Filtering

You may limit the list by providing certain query string parameters to the listing resource. Note, parameters are case-sensitive: 

*   **Log**: Only show notifications for this log, using the integer Log values shown above.
*   **MessageDate**: Only show notifications for this date. Should be formatted as YYYY-MM-DD. You can also specify inequality, such as MessageDate<=YYYY-MM-DD for messages logged at or before midnight on a date, and MessageDate>=YYYY-MM-DD for messages logged at or after midnight on a date. When performing a date range query with day-granularity make sure the right endpoint is midnight of the next day e.g., to construct a range query for all calls on YYYY-MM-DD query MessageDate>=YYYY-MM-DD&MessageDate<YYYY-MM-(DD+1).  Examples: 

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Notifications?Log=1&MessageDate=2009-07-06 HTTP/1.1

Would only show WARNING notifications generated on Jul 06th, 2009. 

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Notifications?Log=1&MessageDate>=2009-07-06 HTTP/1.1

Would only show WARNING notifications generated on or after midnight on Jul 06th, 2009. 

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Notifications?Log=1&MessageDate>=2009-07-06&MessageDate<=2009-07-08 HTTP/1.1

 Would only show WARNING notifications generated between midnight Jul 06th, 2009 and midnight Jul 08th, 2009. 

##### POST 

Not Supported. 

##### PUT 

Not Supported. 

##### DELETE 

Not Supported. 





 [1]: http://www.ietf.org/rfc/rfc2822.txt
 [2]: account
 [3]: /docs/errors/reference
 [4]: request#paging
