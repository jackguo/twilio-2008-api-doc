<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: OutgoingCallerIds

The OutgoingCallerId list resource represents a list of an account's validated outgoing caller ID phone numbers. An OutgoingCallerId instance resource represents a single outgoing caller ID that is validated with Twilio for use when making outgoing calls via the REST API and within the [<Dial>][1] verb.



### OutgoingCallerIds Instance Resource

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/OutgoingCallerIds/{OutgoingCallerIdSid}

#### Resource Properties

Property     | Description
:------------ | :---------|
Sid          | A 34 character string that uniquely identifies this resource.
DateCreated  | The date that this resource was created, given in [RFC 2822][2] format.
DateUpdated  | The date that this resource was last updated, given in [RFC 2822][2] format.
FriendlyName | A human readable descriptive text for this resource, up to 64 characters long. By default, the FriendlyName is a nicely formatted version of the PhoneNumber.
AccountSid   | The id of the account with which this resource is associated.
PhoneNumber  | The 10 digit incoming phone number. Always presented as a 10 digit number, with no "decoration" (like dashes, parentheses, etc.)

#### HTTP Methods

##### GET

Example:

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/OutgoingCallerIds/PNe536dfda7c6184afab78d980cb8cdf43 HTTP/1.1 
~~~
<TwilioResponse> 
	<OutgoingCallerId> 
		<Sid>PNe536dfda7c6184afab78d980cb8cdf43</Sid> 
		<AccountSid>AC35542fc30a091bed0c1ed511e1d9935d</AccountSid> 
		<FriendlyName>My Home Phone Number</FriendlyName> 
		<PhoneNumber>4158675309</PhoneNumber> 
		<DateCreated>Tue, 01 Apr 2008 11:26:32 -0700</DateCreated> 
		<DateUpdated>Tue, 01 Apr 2008 11:26:32 -0700</DateUpdated> 
	</OutgoingCallerId> 
</TwilioResponse>
~~~

##### PUT or POST 

Update the caller id, and return the updated resource. There is only one field that you may update: 

Param        | Optional | Description                                                                                                                                                             
------------ | -------- | -------------------------
FriendlyName | Required | A human readable description of a Caller ID, with maximum length of 64 characters (e.g. 'My Cell Phone'). Defaults to a nicely formatted version of the phone number.

##### DELETE 

Delete the caller id from your account. An HTTP 204 response is returned, with no body.




### OutgoingCallerIds List Resource

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/OutgoingCallerIds

#### HTTP Methods

##### GET

Returns a list of `<OutgoingCallerId>` elements, each representing a Caller ID number validated for your account, under an `<OutgoingCallerIds>` list element, which includes [paging information][3].

Example:
~~~ 
<TwilioResponse>  
	<OutgoingCallerIds page="0" numpages="1" pagesize="50" total="2" start="0" end="0">  
		<OutgoingCallerId>  
			<Sid>PNe536dfda7c6184afab78d980cb8cdf43</Sid>  
			<AccountSid>AC35542fc30a091bed0c1ed511e1d9935d</AccountSid>  
			<FriendlyName>Bob Cell Phone</FriendlyName>  
			<PhoneNumber>4158675309</PhoneNumber>  
			<DateCreated>Tue, 01 Apr 2008 11:26:32 -0700</DateCreated>  
			<DateUpdated>Tue, 01 Apr 2008 11:26:32 -0700</DateUpdated>  
		</OutgoingCallerId>  
		<OutgoingCallerId>  
			<Sid>PNe536dfda7c6DDd455fed980cb83345FF</Sid>  
			<AccountSid>AC35542fc30a091bed0c1ed511e1d9935d</AccountSid>  
			<FriendlyName>Company Main Line</FriendlyName>  
			<PhoneNumber>4158675310</PhoneNumber>  
			<DateCreated>Tue, 01 Apr 2008 11:26:32 -0700</DateCreated>  
			<DateUpdated>Tue, 01 Apr 2008 11:26:32 -0700</DateUpdated>  
		</OutgoingCallerId>  
	</OutgoingCallerIds>   
</TwilioResponse> 	
~~~

List Filtering: you may limit the list by providing certain query string parameters to the listing resource. Note, parameters are case-sensitive: 

*   **PhoneNumber**: Only show the caller id resource that exactly matches this ten digit phone number.
*   **FriendlyName**: Only show the caller id resources that exactly matches this name. For example: 

Example:

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/OutgoingCallerIds?PhoneNumber=4158675309 HTTP/1.1

Would only show the caller id record for phone number (415) 867-5309.

##### POST 

Adds a new CallerID to your account. After making this request, Twilio will return to you a validation code and Twilio will dial the phone number given to perform validation. The code returned must be entered via the phone before the CallerID will be added to your account. The following parameters are accepted:

Parameter | Optional | Description
 ------------ | -------- | ----------------------------------
 PhoneNumber  | Required | The 10 digit phone number, but you can specify it in any readable format, such as 415-555-1212 or (415) 555.1212 or 1-415-555-1212
 FriendlyName | Optional | A human readable description of Caller ID resource, with maximum length 64 characters. Ex.: 'My Cell Phone'. Defaults to a nicely formatted version of the PhoneNumber.
 CallDelay    | Optional | The number of seconds, between 0 and 60, to delay before initiating the Validation Call. Defaults to 0.

This will create a new CallerID validation request within Twilio, which initiates a call to the phone number provided and listens for a validation code. The validation request is represented in the response in a <ValidationRequest> element with the following properties:

Property       | Description
:-------------- | :------
AccountSid     | The account to which the Validation Request belongs.
PhoneNumber    | The 10 digit phone number that is being validated.
FriendlyName   | The friendly name you provided, if any.
ValidationCode | The 6 digit validation code that must be entered via the phone to validate this phone number for Caller ID.

For example, here is a typical request and response: 

> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/OutgoingCallerIds HTTP/1.1
> FriendlyName=My+Home+Phone+Number&PhoneNumber=4158675309 
~~~
<TwilioResponse>  
	<ValidationRequest> 
		<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid>
		<PhoneNumber>4158675309</PhoneNumber>
		<FriendlyName>My Home Phone Number</FriendlyName>
		<ValidationCode>123456</ValidationCode>
	</ValidationRequest> 
</TwilioResponse> 	 
~~~

Typically, you would present that validation code to the user who is trying to validate his or her phone number. If you [Add a Caller ID][4] via the Twilio website, this is exactly what we're doing.

##### PUT 

Not Supported. 

##### DELETE 

Not Supported. 




 [1]: /docs/api/2008-08-01/twiml/dial
 [2]: http://www.ietf.org/rfc/rfc2822.txt
 [3]: request#paging
 [4]: /user/account/phone-numbers/verified
