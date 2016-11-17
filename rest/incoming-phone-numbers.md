<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: IncomingPhoneNumbers

The IncomingPhoneNumber list resource represents a list of your account's Twilio numbers. An IncomingPhoneNumber instance resource represents a single phone number that you can use to receive incoming phone calls.



### IncomingPhoneNumber Instance Resource

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/IncomingPhoneNumbers/{IncomingPhoneNumberSid}

This resource represents an individual phone number belonging to an account.

#### Resource Properties

 Property     | Description
:------------ |:----------------
 Sid          | A 34 character string that uniquely identifies this resource.
 DateCreated  | The date that this resource was created, given in [RFC 2822][13] format.
 DateUpdated  | The date that this resource was last updated, given in [RFC 2822][13] format.
 FriendlyName | A human readable descriptive text for this resource, up to 64 characters long. By default, the FriendlyName is a nicely formatted version of the PhoneNumber. |
 AccountSid   | The id of the account with which this resource is associated.
 PhoneNumber  | The 10 digit incoming phone number. Always presented as a 10 digit number, with no "decoration" (like dashes, parentheses, etc.)
 VoiceCallerIdLookup | Look up the Caller's caller-ID name from the CNAM database (additional charges apply). Either `true` or `false`.
 Url          | The URL Twilio will request when phone number is called.
 Method       | The HTTP method to use when requesting the above URL. Either GET or POST.
 VoiceFallbackUrl | The URL that Twilio will request if an error occurs retrieving or executing the TwiML requested by "Url".
 VoiceFallbackMethod | The HTTP method to use when requesting the voice fallback URL. Either GET or POST.
 SmsUrl       | The URL Twilio will request when receiving an incoming SMS message to this number.
 SmsMethod    | The HTTP method to use when requesting the sms  URL. Either GET or POST.
 SmsFallbackUrl| The URL that Twilio will request if an error occurs retrieving or executing the "SmsUrl" TwiML.
 SmsFallbackMethod| The HTTP method to use when requesting the above URL. Either GET or POST.

#### HTTP Methods

##### GET

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/IncomingPhoneNumbers/PNe536dfda7c6184afab78d980cb8cdf43 HTTP/1.1 
~~~
<TwilioResponse> 
	<IncomingPhoneNumber> 
		<Sid>PNe536dfda7c6184afab78d980cb8cdf43</Sid> 
		<AccountSid>AC35542fc30a091bed0c1ed511e1d9935d</AccountSid> 
		<FriendlyName>My Home Phone Number</FriendlyName> 
		<PhoneNumber>4158675309</PhoneNumber> 
		<Url>http://mycompany.com/handleMainLineCall.asp</Url>
		<Method>GET</Method>
		<SmsUrl>http://mycompany.com/handleIncomingSms.asp</SmsUrl>
		<SmsMethod>POST</SmsMethod>
		<VoiceFallbackUrl>http://mycompany.com/handleError.asp</VoiceFallbackUrl>
		<VoiceFallbackMethod>POST</VoiceFallbackMethod>
		<SmsFallbackUrl>http://mycompany.com/handleSmsError.asp</SmsFallbackUrl>
		<SmsFallbackMethod>POST</SmsFallbackMethod>
		<VoiceCallerIdLookup>false</VoiceCallerIdLookup>
		<DateCreated>Tue, 01 Apr 2008 11:26:32 -0700</DateCreated> 
		<DateUpdated>Tue, 01 Apr 2008 11:26:32 -0700</DateUpdated> 
	</IncomingPhoneNumber> 
</TwilioResponse>
~~~
##### PUT or POST 

Update the incoming phone number, and return the updated resource. You may POST the following properties to update the number: 

"IncomingPhoneNumber POST Parameters"
 Param        | Optional | Description
 :------------ | :-------- | ----------------------------------------------------- |
 FriendlyName | Optional | A human readable description of the new incoming phone number resource, with maximum length 64 characters. Ex.: 'Company Support Line'. 
 Url          | Required | The URL that Twilio should request when somebody dials the phone number. Ex.: 'http://mycompany.com/handleNewCall.php'
 Method       | Optional | The HTTP method that should be used to request the URL. Must be either GET or POST. Ex.: 'GET'. Defaults to POST. 
 VoiceFallbackUrl | Optional | A URL that Twilio will request if an error occurs requesting or executing the TwiML defined by "Url". Ex : 'http://mycompany.com/errorHandler.php' |
 VoiceFallbackMethod | Optional | The HTTP method that should be used to request the VoiceFallbackUrl. Must be either GET or POST. Ex.: 'GET'. Defaults to POST. |
 SmsUrl | Optional | The URL that Twilio should request when somebody sends an SMS to the new phone number.  Ex: 'http://mycompany.com/handleSms.php' |
 SmsMethod | Optional | The HTTP method that should be used to request the SmsUrl. Must be either GET or POST. Ex.: 'GET'. Defaults to POST. |
 SmsFallbackUrl | Optional | A URL that Twilio will request if an error occurs requesting or executing the TwiML defined by "SmsUrl". Ex : 'http://mycompany.com/errorHandler.php' |
 SmsFallbackMethod | Optional | The HTTP method that should be used to request the SmsFallbackUrl. Must be either GET or POST. Ex.: 'GET'. Defaults to POST. |
 VoiceCallerIdLookup | Optional | Do a lookup of a caller's name from the CNAM database and post it to your app. Either `true` or `false`. |

##### DELETE 

Release this phone number from your account. Twilio will no longer answer calls to this number and will stop billing you. Twilio will eventually recycle the phone number and potentially give it to another customer, so delete with care. If you make a mistake, contact us. We may be able to return the number to you.





### IncomingPhoneNumbers List Resource

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/IncomingPhoneNumbers 

#### HTTP Methods

##### GET 

Returns a set of `<IncomingPhoneNumber>` elements, each representing a phone number given to your account, under an `<IncomingPhoneNumbers>` list element that includes [paging information][14]. 

Example: 

~~~
<TwilioResponse>  
	<IncomingPhoneNumbers page="0" numpages="1" pagesize="50" total="2" start="0" end="0">  
		<IncomingPhoneNumber>  
			<Sid>PNe536dfda7c6184afab78d980cb8cdf43</Sid>  
			<AccountSid>AC35542fc30a091bed0c1ed511e1d9935d</AccountSid>  
			<FriendlyName>Company Main Line</FriendlyName>  
			<PhoneNumber>4158675309</PhoneNumber> 
			<Url>http://mycompany.com/handleMainLineCall.asp</Url>
			<Method>GET</Method>
			<SmsUrl>http://mycompany.com/handleIncomingSms.asp</SmsUrl>
			<SmsMethod>POST</SmsMethod>
			<VoiceFallbackUrl>http://mycompany.com/handleError.asp</VoiceFallbackUrl>
			<VoiceFallbackMethod>POST</VoiceFallbackMethod>
			<SmsFallbackUrl>http://mycompany.com/handleSmsError.asp</SmsFallbackUrl>
            <SmsFallbackMethod>POST</SmsFallbackMethod>
			<VoiceCallerIdLookup>false</VoiceCallerIdLookup>
			<DateCreated>Tue, 01 Apr 2008 11:26:32 -0700</DateCreated>  
			<DateUpdated>Tue, 01 Apr 2008 11:26:32 -0700</DateUpdated>  
		</IncomingPhoneNumber>  
		<IncomingPhoneNumber>  
			<Sid>PNe536dfda7c6DDd455fed980cb83345FF</Sid>  
			<AccountSid>AC35542fc30a091bed0c1ed511e1d9935d</AccountSid>  
			<FriendlyName>Voice Test Line, No SMS or Fallback configured</FriendlyName>  
			<PhoneNumber>4158675310</PhoneNumber>  
			<Url>http://mycompany.com/handleSupportCall.php</Url>
			<Method>POST</Method>
			<SmsUrl></SmsUrl>
            <SmsMethod></SmsMethod>
            <VoiceFallbackUrl></VoiceFallbackUrl>
            <VoiceFallbackMethod></VoiceFallbackMethod>
            <SmsFallbackUrl></SmsFallbackUrl>
            <SmsFallbackMethod></SmsFallbackMethod>
            <VoiceCallerIdLookup>true</VoiceCallerIdLookup>
			<DateCreated>Tue, 01 Apr 2008 11:26:32 -0700</DateCreated>  
			<DateUpdated>Tue, 01 Apr 2008 11:26:32 -0700</DateUpdated>  
		</IncomingPhoneNumber>  
	</IncomingPhoneNumbers> 

</TwilioResponse>
~~~

##### List Filtering

You may limit the list by providing certain query string parameters to the listing resource. Note, parameters are case-sensitive:
 
*   **PhoneNumber**: Only show the incoming phone number resource that exactly matches this ten digit phone number.
*   **FriendlyName**: Only show the incoming phone number resource that exactly matches this name. For example: 

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/IncomingPhoneNumbers?PhoneNumber=4158675309 HTTP/1.1

Would only show the record for phone number (415) 867-5309. 

##### POST 

Not Supported. 

##### PUT 

Not Supported. 

##### DELETE 

Not Supported. 

### Local IncomingPhoneNumber Factory Resource 

This sub-resource represents only Local phone numbers, i.e. not toll-free numbers. POSTing to this resource allows you to request a new local phone number be added to your account.

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/IncomingPhoneNumbers/Local

#### HTTP Methods

##### GET 

Returns a list of local `<IncomingPhoneNumber>` elements, each representing a local (not toll-free) phone number given to your account, under an `<IncomingPhoneNumbers>` list element that includes [paging information][14]. Works exactly the same as the IncomingPhoneNumber resource, but filters out toll-free numbers. 

##### POST

Allows you to add a new local phone number to your account. If a phone number is found for your request, Twilio will add it to your account and bill you for the first month's cost of the phone number. If Twilio can't find a phone number to match your request, you will receive an HTTP 400 with [Twilio error code 21452][15].

<div class="alert alert-error">
NOTE: you can only request phone numbers with a full Twilio account, not in the Twilio Free Trial. If you would like to buy a Twilio phone number, you must upgrade your account.
</div>

The following POST parameters are accepted:

"IncomingPhoneNumber POST Parameters"
Param        | Optional | Description
:------------ | :-------- | :--------------------------------|
 AreaCode     | Optional | The area code in which you'd like a new incoming phone number. Any three digit, US area code is valid. If not supplied, Twilio will find a phone number anywhere in the United States.            |
 FriendlyName | Optional | A human readable description of the new incoming phone number resource, with maximum length 64 characters. Ex.: 'Company Support Line'. Defaults to a nicely formatted version of the PhoneNumber. |
 Url          | Optional | The URL that Twilio should request when somebody dials the new phone number. Ex.: 'http://mycompany.com/handleNewCall.php' |
 Method       | Optional | The HTTP method that should be used to request the URL. Must be either GET or POST. Ex.: 'GET'. Defaults to POST. |
 VoiceFallbackUrl | Optional | A URL that Twilio will request if an error occurs requesting or executing the TwiML defined by "Url". Ex : 'http://mycompany.com/errorHandler.php' |
 VoiceFallbackMethod | Optional | The HTTP method that should be used to request the VoiceFallbackUrl. Must be either GET or POST. Ex.: 'GET'. Defaults to POST. |
 SmsUrl | Optional | The URL that Twilio should request when somebody sends an SMS to the new phone number.  Ex: 'http://mycompany.com/handleSms.php' |
 SmsMethod | Optional | The HTTP method that should be used to request the SmsUrl. Must be either GET or POST. Ex.: 'GET'. Defaults to POST. |
 SmsFallbackUrl | Optional | A URL that Twilio will request if an error occurs requesting or executing the TwiML defined by "SmsUrl". Ex : 'http://mycompany.com/errorHandler.php' |
 SmsFallbackMethod | Optional | The HTTP method that should be used to request the SmsFallbackUrl. Must be either GET or POST. Ex.: 'GET'. Defaults to POST. |
 VoiceCallerIdLookup | Optional | Do a lookup of a caller's name from the CNAM database and post it to your app. Either `true` or `false`. Defaults to `false`. |
 
If successful, Twilio responds with a representation of the new phone number assigned to your account. Example: 

> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/IncomingPhoneNumbers/Local HTTP/1.1
> AreaCode=415&Url=http://mycompany.com/handleNewCall.php&Method=GET&FriendlyName=My+Company+Line
~~~
<TwilioResponse> 
	<IncomingPhoneNumber> 
		<Sid>PNe536dfda7c6184afab78d980cb8cdf43</Sid> 
		<AccountSid>AC35542fc30a091bed0c1ed511e1d9935d</AccountSid> 
		<FriendlyName>My Company Line</FriendlyName> 
		<PhoneNumber>4158675309</PhoneNumber> 
		<Url>http://mycompany.com/handleNewCall.php</Url>
		<Method>GET</Method>
		<SmsUrl>http://mycompany.com/handleIncomingSms.asp</SmsUrl>
		<SmsMethod>POST</SmsMethod>
		<VoiceFallbackUrl>http://mycompany.com/handleError.asp</VoiceFallbackUrl>
		<VoiceFallbackMethod>POST</VoiceFallbackMethod>
		<SmsFallbackUrl>http://mycompany.com/handleSmsError.asp</SmsFallbackUrl>
		<SmsFallbackMethod>POST</SmsFallbackMethod>
		<VoiceCallerIdLookup>false</VoiceCallerIdLookup>
		<DateCreated>Tue, 01 Apr 2008 11:26:32 -0700</DateCreated> 
		<DateUpdated>Tue, 01 Apr 2008 11:26:32 -0700</DateUpdated> 
	</IncomingPhoneNumber> 
</TwilioResponse>	
~~~

##### PUT 

Not Supported. 

##### DELETE 

Not Supported. 

### TollFree IncomingPhoneNumber Factory Resource 

This sub-resource represents only toll-free phone numbers, i.e. not local numbers. POSTing to this resource allows you to request a new toll-free phone number be added to your account. 

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/IncomingPhoneNumbers/TollFree 

#### HTTP Methods

##### GET 

Returns a list of toll free `<IncomingPhoneNumber>` elements, each representing a toll-free (not local) phone number given to your account, under an `<IncomingPhoneNumbers>` list element, which includes [paging information][14]. Works exactly the same as the IncomingPhoneNumber resource, but filters out non-toll-free numbers. 

##### POST

Allows you to add a new toll free phone number to your account. If a phone number is found for your request, Twilio will add it to your account and bill you for the first month's cost of the phone number. If Twilio can't find a phone number to match your request, you will receive an HTTP 400 with [Twilio error code 21452][15].

<div class="alert alert-error">
NOTE: you can only request phone numbers with a full Twilio account, not in the Twilio Free Trial. If you would like to buy a Twilio phone number, you must upgrade your account.
</div>

The following POST parameters are accepted:

"IncomingPhoneNumber POST Parameters"
Param        | Optional | Description
 :------------ | :-------- | :--------------------------------- |
 AreaCode     | Optional | The area code in which you'd like a new incoming phone number. Any three digit, US area code is valid. If not supplied, Twilio will find a phone number anywhere in the United States.            |
 FriendlyName | Optional | A human readable description of the new incoming phone number resource, with maximum length 64 characters. Ex.: 'Company Support Line'. Defaults to a nicely formatted version of the PhoneNumber. |
 Url          | Optional | The URL that Twilio should request when somebody dials the new phone number. Ex.: 'http://mycompany.com/handleNewCall.php'
 Method       | Optional | The HTTP method that should be used to request the URL. Must be either GET or POST. Ex.: 'GET'. Defaults to POST.
 VoiceFallbackUrl | Optional | A URL that Twilio will request if an error occurs requesting or executing the TwiML defined by "Url". Ex : 'http://mycompany.com/errorHandler.php'
 VoiceFallbackMethod | Optional | The HTTP method that should be used to request the VoiceFallbackUrl. Must be either GET or POST. Ex.: 'GET'. Defaults to POST.
 VoiceCallerIdLookup | Optional | Do a lookup of a caller's name from the CNAM database and post it to your app. Either `true` or `false`. Defaults to `false`. |
 
 
If successful, Twilio responds with a representation of the new phone number assigned to your account. Example: 

> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/IncomingPhoneNumbers/TollFree HTTP/1.1
> AreaCode=866&Url=http://mycompany.com/handleNewCall.php&Method=GET&FriendlyName=My+Company+Line
~~~
<TwilioResponse> 
	<IncomingPhoneNumber> 
		<Sid>PNe536dfda7c6184afab78d980cb8cdf43</Sid> 
		<AccountSid>AC35542fc30a091bed0c1ed511e1d9935d</AccountSid> 
		<FriendlyName>My Company Line</FriendlyName> 
		<PhoneNumber>8668675309</PhoneNumber> 
		<Url>http://mycompany.com/handleNewCall.php</Url>
		<Method>GET</Method>
		<VoiceFallbackUrl>http://mycompany.com/handleError.asp</VoiceFallbackUrl>
		<VoiceFallbackMethod>POST</VoiceFallbackMethod>
		<VoiceCallerIdLookup>false</VoiceCallerIdLookup>
		<DateCreated>Tue, 01 Apr 2008 11:26:32 -0700</DateCreated> 
		<DateUpdated>Tue, 01 Apr 2008 11:26:32 -0700</DateUpdated> 
	</IncomingPhoneNumber> 
</TwilioResponse>	
~~~
##### PUT 

Not Supported. 

##### DELETE 

Not Supported. 





 [1]: index
 [2]: request
 [3]: response
 [4]: account
 [5]: outgoing-caller-ids
 [6]: incoming-phone-numbers
 [7]: call
 [8]: making_calls
 [9]: recording
 [10]: transcription
 [11]: notification
 [12]: tips
 [13]: http://www.ietf.org/rfc/rfc2822.txt
 [14]: request#paging
 [15]: /docs/errors/21452
