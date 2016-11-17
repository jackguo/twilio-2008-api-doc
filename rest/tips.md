<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: Tips & Tricks

### Vanity URLs

You can create a CNAME of our API URLS and serve up recording media from a hostname of your choosing!  To do this, create a CNAME record with your DNS provider that points to api.twilio.com.  Note, you shouldn't use SSL (https) URLs if you CNAME, as the certificate will not match.

### Alternate Representations

#### CSV 

Append a .csv to the end of REST URLs, and you'll get a [Comma Separated Values (CSV)][1]
 representation of the data, with a header row describing the columns. 
 For example, if you requested the following Calls resource, the default 
 representation is XML: 
 
> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls HTTP/1.1 

~~~
<TwilioResponse>  
	<Calls page="0" numpages="1" pagesize="50" total="38" start="0" end="37">  
		<Call>  
			<Sid>CA42ed11f93dc08b952027ffbc406d0868</Sid>  
			<DateCreated>Sat, 07 Feb 2009 13:15:19 -0800</DateCreated>
			<DateUpdated>Sat, 07 Feb 2009 13:15:19 -0800</DateUpdated>
			<CallSegmentSid/>  
			<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid>  
			<Called>4159633717</Called>  
			<Caller>4156767925</Caller>  
			<PhoneNumberSid>PN012345678901246789012345678900</PhoneNumberSid>  
			<Status>2</Status>  
			<StartTime>Thu, 03 Apr 2008 04:36:33 -0400</StartTime>  
			<EndTime>Thu, 03 Apr 2008 04:36:47 -0400</EndTime>  
			<Duration>14</Duration>  
			<Price/>  
			<Flags>1</Flags>  
		</Call>  
		<Call>  
			<Sid>CA751e8fa0a0105cf26a0d7a9775fb4bfb</Sid>  
			<DateCreated>Sat, 07 Feb 2009 13:15:19 -0800</DateCreated>
			<DateUpdated>Sat, 07 Feb 2009 13:15:19 -0800</DateUpdated>
			<CallSegmentSid/>  
			<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid>  
			<Called>2064287985</Called>  
			<Caller>4156767925</Caller>  
			<PhoneNumberSid>PNd59c2ba27ef482647db90476d1674</PhoneNumberSid>  
			<Status>2</Status>  
			<StartTime>Thu, 03 Apr 2008 01:37:05 -0400</StartTime>  
			<EndTime>Thu, 03 Apr 2008 01:37:40 -0400</EndTime>  
			<Duration>35</Duration>  
			<Price/>  
			<Flags>1</Flags>  
		</Call>  
		... 
	</Calls> 
</TwilioResponse>
~~~

Appending a .csv, you'd get a CSV representation of the same data with mime-type 'text/csv'. Each Call is a row in the table, and each XML element a column: 

>GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls.csv HTTP/1.1 

Would return: 

>Sid,DateCreated,DateUpdated,CallSegmentSid,AccountSid,Called,Caller,...
>CA42ed11f93dc08b952027ffbc406d0868,"Sat, 07 Feb 2009 13:15:19 -0800",
"Sat, 07 Feb 2009 13:15:19 -0800",,AC309475e5fede1b49e100272a8640f438,
4159633717,4156767925,PN01234567890123456789012345678900,2,"Thu, 
03 Apr 2008 04:36:33 -0400","Thu, 03 Apr 2008 04:36:47 -0400",14,,1,, 

#### JSON

You can also request JSON responses by appending a .json to the end of any REST URL.  The response mime-type is 'application/json'.

For example, to retrieve Call records as JSON: 
 
> GET https://api.twilio.com/2008-08-01/Accounts/AC35542fc30a091bed0c1ed511e1d9935d/Calls.json HTTP/1.1 

~~~
{"TwilioResponse":{
	"Calls":[{
		"Call":{
			"Sid":"CA42ed11f93dc08b952027ffbc406d0868",
			"DateCreated":"Thu, 18 Feb 2009 16:24:27 -0800",
			"DateUpdated":"Thu, 18 Feb 2009 16:24:35 -0800",
			"CallSegmentSid":"",
			"AccountSid":"AC35542fc30a091bed0c1ed511e1d9935d",
			"Called":"4158675309",
			"Caller":"4155551212",
			"PhoneNumberSid":"PNeeba9b7a16d3fe27cacc0402d95e90d6",
			"Status":"2",
			"StartTime":"Thu, 18 Feb 2009 16:24:27 -0800",
			"EndTime":"Thu, 18 Feb 2009 16:24:35 -0800",
			"Duration":"8",
			"Price":"-0.03000",
			"Flags":"1",
			"Annotation":null
			}
		},{"Call":{
			"Sid":"CA751e8fa0a0105cf26a0d7a9775fb4bfb",
			"DateCreated":"Thu, 18 Feb 2009 16:23:40 -0800",
			"DateUpdated":"Thu, 18 Feb 2009 16:23:44 -0800",
			"CallSegmentSid":"",
			"AccountSid":"AC35542fc30a091bed0c1ed511e1d9935d",
			"Called":"4158675309",
			"Caller":"4155551212",
			"PhoneNumberSid":"PNeeba9b7a16d3fe27cacc0402d95e90d6",
			"Status":"2",
			"StartTime":"Thu, 18 Feb 2009 16:23:40 -0800",
			"EndTime":"Thu, 18 Feb 2009 16:23:44 -0800",
			"Duration":"4",
			"Price":"-0.03000",
			"Flags":"1",
			"Annotation":null
			}
		}}]
	}
}
~~~



#### HTML 

You can also get Twilio to return the same objects formatted as HTML by appending a .html to the end of any url. This may be useful if you intend to pass this data directly to a browser, although we kinda doubt it.  Note the following differences:
  
*   **Elements:** The elements become `<div>`s, and the element name (ex. Account) becomes the class name in lower case, using the dash notation instead of CamelCase. For example, 'account-sid' instead of 'AccountSid'
*   **Body:** The body is given class 'twilio-response'
*   **Title:** The title is the http response code. For example, 200.

For example, consider the following XML representation of an account:  
  
> GET https://api.twilio.com/2008-08-01/Accounts/AC35542fc30a091bed0c1ed511e1d9935d HTTP/1.1 
~~~
<TwilioResponse> 
	<Account> 
		<Sid>AC35542fc30a091bed0c1ed511e1d9935d</Sid> 
		<DateCreated>Sat, 07 Feb 2009 13:15:19 -0800</DateCreated>
		<DateUpdated>Sat, 07 Feb 2009 13:15:19 -0800</DateUpdated>
		<FriendlyName>Friendly Account 1</FriendlyName> 
		<Status>1</Status> 
		<StatusText>Inactive</StatusText> 
		<DateCreated>Tue, 01 Apr 2008 11:26:32 -0700</DateCreated> 
		<DateUpdated>Tue, 01 Apr 2008 11:26:32 -0700</DateUpdated> 
	</Account> 
</TwilioResponse>	
~~~
  
The same resource, represented as HTML is shown below. Note the .html appended to the URL:  
  
> GET https://api.twilio.com/2008-08-01/Accounts/AC35542fc30a091bed0c1ed511e1d9935d.html HTTP/1.1 
~~~
<html> 
	<head><title>200</title></head> 
	<body class="twilio-response"> 
		<div class="account"> 
			<div class="sid">AC35542fc30a091bed0c1ed511e1d9935d</div> 
			<div class='date-created'>Sat, 07 Feb 2009 13:15:19 -0800</div>
			<div class='date-updated'>Sat, 07 Feb 2009 13:15:19 -0800</div>
			<div class="friendly-name">Friendly Account 1</div> 
			<div class="status">1</div> 
			<div class="status-text">Inactive</div> 
			<div class="date-created">Tue, 01 Apr 2008 11:26:32 -0700</div> 
			<div class="date-updated">Tue, 01 Apr 2008 11:26:32 -0700</div> 
			<div class="auth-token">47a2f3c53df26a8b9d79bdf84406527a</div> 
		</div> 
	</body> 
</html> 
~~~

 [1]: http://en.wikipedia.org/wiki/Comma-separated_values
