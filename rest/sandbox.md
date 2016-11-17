<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# Sandbox Resource

<div class="alert alert-error">
The Sandbox resource has been deprecated and may be removed in future versions
of the Twilio API.
</div>

The Sandbox resource gives you programmatic access to your Twilio Developer
Sandbox phone number. Using this resource, you can get the phone number and PIN
for your sandbox, view the current voice and SMS URLs, and update those URLs
just like any other [Incoming Phone Number][IncominmgPhoneNumber] on a Full
Twilio Account.

[IncominmgPhoneNumber]: incoming-phone-numbers

### Base Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/Sandbox

### Resource Properties

A `<TwilioSandbox>` resource is represented by the following properties:

"Call Resource Properties"
Property  | Description
:-------------	|:-------------
Pin  |	An 8 digit number that gives access to this sandbox.
AccountSid    | The 34 character id of the [Account][account] this notification is associated with. (Your account!)
PhoneNumber |	The 10 digit phone number where you can call into the sandbox.
Url          | The URL Twilio will request when this sandbox is called.                                                                                                      
Method       | The HTTP method to use when requesting the above URL. Either GET or POST.  
SmsUrl       | The URL Twilio will request when receiving an incoming SMS message to this sandbox.  
SmsMethod    | The HTTP method to use when requesting the sms URL. Either GET or POST.  
DateCreated  |	The date that this resource was created, given in RFC 2822 format.
DateUpdated  |	The date that this resource was last updated, given in RFC 2822 format.

[account]: account

## Sandbox Instance Resource

### HTTP Methods

#### GET

Returns the Sandbox associated with this account.  Twilio accounts upgraded prior to February 2010 may not have a sandbox, and in this case, you will receive a 404 (Not Found) response.

>GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Sandbox HTTP/1.1 

~~~
<TwilioResponse>  
	<TwilioSandbox>  
		<Pin>123456</Pin>  
		<PhoneNumber>5558675309</PhoneNumber>  
		<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid>  
		<DateCreated>Sat, 07 Feb 2009 13:15:19 -0800</DateCreated>
		<DateUpdated>Sat, 07 Feb 2009 13:15:19 -0800</DateUpdated>
		<Url>http://mycompany.com/handle-sandbox-call.asp</Url>
		<Method>GET</Method>
		<SmsUrl>http://mycompany.com/handle-sandbox-sms.asp</SmsUrl>
		<SmsMethod>POST</SmsMethod>
	</TwilioSandbox>  
</TwilioResponse>  
~~~

#### POST

You can POST to the Sandbox resource to update the application URLs associated with your sandbox.

Your request may include the following parameters:

"Sandbox POST Parameters"
Param        | Optional | Description                                                                                                                                                                                       
:------------ | :-------- | :--------------------------------|
 Url          | Optional | The URL that Twilio should request when somebody calls this sandbox. Ex.: 'http://mycompany.com/handleNewCall.php' |
 Method       | Optional | The HTTP method that should be used to request the URL. Must be either GET or POST. Ex.: 'GET'. Defaults to POST. |
 SmsUrl | Optional | The URL that Twilio should request when somebody sends an SMS to the sandbox.  Ex: 'http://mycompany.com/handleSms.php' |
 SmsMethod | Optional | The HTTP method that should be used to request the SmsUrl. Must be either GET or POST. Ex.: 'GET'. Defaults to POST. |
 
If successful, Twilio responds with an updated representation of the sandbox. Example: 

> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Sandbox HTTP/1.1 
    	Url=http://mynewdomain.com/my-call-handler.php&Method=POST
~~~
<TwilioResponse>  
	<TwilioSandbox>  
		<Pin>123456</Pin>  
		<PhoneNumber>5558675309</PhoneNumber>  
		<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid>  
		<DateCreated>Sat, 07 Feb 2009 13:15:19 -0800</DateCreated>
		<DateUpdated>Sat, 07 Feb 2009 13:15:19 -0800</DateUpdated>
		<Url>http://mynewdomain.com/my-call-handler.php</Url>
		<Method>POST</Method>
		<SmsUrl>http://mycompany.com/handle-sandbox-sms.asp</SmsUrl>
		<SmsMethod>POST</SmsMethod>
	</TwilioSandbox>  
</TwilioResponse>  
~~~
 

#### PUT

Same as POST, see above.

#### DELETE

Not supported
