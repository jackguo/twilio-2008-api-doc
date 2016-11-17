<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: Calls

The Calls list resource represents a list of phone calls made to and from an account.
A Call resource represents a connection between a telephone and Twilio. This may 
be inbound, when a person calls your application, or outbound, when your 
application initiates the call, either via the REST API (see [Making Calls][rest]) or 
during a call via the [`<Dial>`][dial] verb.

[dial]: /docs/api/2008-08-01/twiml/dial
[rest]: making_calls


##### Call Segments

A single call may contain multiple call "Segments", each represented as a 
Call element. For example, if a person dials your application, that's one 
call record. Then, if your application [`<Dial>`][dial]s another phone number, a new 
related call segment is created. Both have the same Sid, but different 
CallSegmentSids. For the initial Call, the CallSegmentSid is empty.




### Call Instance Resource

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/Calls/{CallSid} 

This resource represents an individual phone call. For calls with multiple segments, this represents the initial phone call. Calls made via the [`<Dial>`][dial] verb are available via the [Segments subresource][seg]. 

[seg]: call#segments-subresource

#### Resource Properties

A Call resource is represented by the following properties:

"Call Resource Properties"
Property  | Description
:-------------	|:-------------
Sid  |	A 34 character string that uniquely identifies this resource.
CallSegmentSid |	A 34 character string that uniquely identifies a portion of a phone call. For example, if you use the [`<Dial>`][dial] verb during a call, you've created a new call segment. CallSegmentSid is an empty string for the initial call.
DateCreated  |	The date that this resource was created, given in RFC 2822 format.
DateUpdated  |	The date that this resource was last updated, given in RFC 2822 format.
AccountSid 	 | The 34 character id of the [Account][account] this Call is associated with. (Your account!)
Called 		| The phone number of the telephone that received this Call. For incoming calls, it's one of your Twilio phone numbers. For outgoing calls, it's the person that you called. Always presented as a 10 digit number, with no "decoration" (like dashes, parentheses, etc.)
Caller 	| The Caller ID of the telephone that made this Call. For incoming calls, it's the person who called your Twilio phone number. For outgoing calls, it's the Caller you specified for the call. Always presented as a 10 digit number, with no "decoration" (like dashes, parentheses, etc.)
PhoneNumberSid 	| If the call was inbound, this is the Sid of the IncomingPhoneNumber that received the call. If the call was outgoing, it is the Sid of the OutgoingCallerId from which the call was placed.
Status |	An integer representing the status of the call. 0 = Not Yet Dialed, 1 = In Progress, 2 = Complete, 3 = Failed - Busy, 4 = Failed - Application Error, 5 = Failed - No Answer
StartTime |	The timestamp of the start of the call, given in RFC 2822 format. If the call has not yet been dialed, this field will be empty.
EndTime |	The timestamp of the end of the call, given in RFC 2822 format. If the call did not complete successfully, this field will be empty.
Duration |	Duration is the length of the call in seconds. This value is only populated for successfully completed calls. This will be an empty value for calls that are currently ongoing, that were busy, failed, or were not answered.
Price |	The charge for this call in USD. Populated after the call is completed. Note, this value may not be immediately available. In a trial account, this value is not populated, since the call was free!
Flags |	A bitwise field for storing extra information about a call. Possible values are: 1 means the call was inbound, 2 means the call was initiated by the API, 4 means the call segment was initiated by a Dial verb. These values may be OR'd together. For example, if you initiated a call via the API, the flags would equal 2. Then, if during that call you used the <Dial> verb to call another person, the second call segment would have Flag equal 6, indicating the the call was originally API initiated, and then the segment was Dial initiated. 

[account]: account


#### HTTP Methods

##### GET

Returns a single Call resource with the Sid provided. Example:

>GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls/CA42ed11f93dc08b952027ffbc406d0868 HTTP/1.1

~~~
<TwilioResponse>  
	<Call>  
		<Sid>CA42ed11f93dc08b952027ffbc406d0868</Sid>  
		<DateCreated>Sat, 07 Feb 2009 13:15:19 -0800</DateCreated>
		<DateUpdated>Sat, 07 Feb 2009 13:15:19 -0800</DateUpdated>
		<CallSegmentSid/>  
		<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid>  
		<Called>4159633717</Called>  
		<Caller>4156767925</Caller> 
		<PhoneNumberSid>PN01234567890123456789012345678900</PhoneNumberSid>  
		<Status>2</Status>  
		<StartTime>Thu, 03 Apr 2008 04:36:33 -0400</StartTime>  
		<EndTime>Thu, 03 Apr 2008 04:36:47 -0400</EndTime>  
		<Duration>14</Duration>  
		<Price/>  
		<Flags>1</Flags>  
	</Call> 
</TwilioResponse> 	
~~~

##### POST

Initiates a call redirect. See [Redirecting Calls][redirect] for full details.
[redirect]: change-call-state

##### PUT

Not supported

##### DELETE

Not supported

#### Call Instance Sub Resources

The Call resource has a few sub-resources for your convenience.

##### Segments

> /2008-08-01/Accounts/{YourAccountSid}/Calls/{CallSid}/Segments

Returns a list of Call resources that were segments created during the call with the given CallSid.

##### A Segment Instance

> /2008-08-01/Accounts/{YourAccountSid}/Calls/{CallSid}/Segments/{CallSegmentSid}

Returns a single Call resource for the CallSid and CallSegmentSid provided.

##### Recordings

> /2008-08-01/Accounts/{YourAccountSid}/Calls/{CallSid}/Recordings

Returns a list of recordings generated during the call with the given CallSid. See the [Recordings][record] section for full details.

##### Notifications

> /2008-08-01/Accounts/{YourAccountSid}/Calls/{CallSid}/Notifications

Returns a list of notifications generated during the call with the given CallSid. See the [Notifications][notify] section for full details.


### Calls List Resource

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/Calls

#### HTTP Methods

##### GET

The Calls list resource is represented by individual `<Call>` elements under a `<Calls>` list element that includes [paging information][page]. The list is sorted by DateUpdated, with newest calls first. Example: 

[page]: response#paging

>GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls HTTP/1.1 
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
			<PhoneNumberSid>PN01234567890123456789012345678900</PhoneNumberSid>  
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
			<PhoneNumberSid>PNd59c2ba27ef48264773edb90476d1674</PhoneNumberSid>  
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

##### List Filtering

You may limit the list returned by providing certain query string parameters when GETting the list resource. Note, parameters are case-sensitive:

* **Called:** Only show calls to this ten digit phone number.
* **Caller:** Only show calls from this ten digit phone number.
* **Status:** Integer between 0 and 2. Only show calls currently in this status.
* **StartTime:** Only show calls that started on this date, given as YYYY-MM-DD. Example: StartTime=2009-07-06. You can also specify inequality, such as StartTime<=YYYY-MM-DD for calls that started at or before midnight on a date, and StartTime>=YYYY-MM-DD for calls that started at or after midnight on a date.

Examples:

> /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls?Status=1&StartTime=2009-07-06

Would only show in progress calls started on Jul 06th, 2009.

> /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls?Status=1&StartTime>=2009-07-06

Would only show in progress calls started on or after midnight Jul 06th, 2009.

> /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls?Status=1&StartTime>=2009-07-04&StartTime<=2009-07-06

Would only show in progress calls started between midnight Jul 04th, 2009 and midnight Jul 06th, 2009. 

##### POST

Initiates a new outbound phone call. See [Making Calls][calls] for full details.

[calls]: making_calls

##### PUT

Not supported

##### DELETE

Not supported

##### Tip: Get a CSV of your Calls

Did you know you can append the extension '.csv' to any REST resource to get a comma separated values representation? This is especially useful for call logs. Try this:

> GET /2008-08-01/Accounts/{YourAccountSid}/Calls.csv HTTP/1.1

Check out the [Tips & Tricks Section][tips] for more about alternate representations, including CSV.

[tips]: tips
[record]: recording
[notify]: notification
