<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: Accounts

The Accounts list resource represents a list of accounts. An Account instance resource represents a single Twilio Account.



### Account Instance Resource

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}

#### Resource Properties

An Account resource is represented by the following properties:

Property  | Description
:-------------	|:-------------
Sid 			|A 34 character string that uniquely identifies this account.
DateCreated 	|The date that this account was created, given in RFC 2822 format.
DateUpdated 	|The date that this account was last updated, given in RFC 2822 format.
FriendlyName 	|A human readable description of this account, up to 64 characters long. By default the FriendlyName is your email address.
Status 			|An integer representing the status of this account. 2 is Active. 4, 16, and 32 are Suspended.

#### HTTP Methods

##### GET

Returns a representation of your account, including the properties above. For Example:

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438 HTTP/1.1

~~~
<TwilioResponse>
	<Account>
		<Sid>AC309475e5fede1b49e100272a8640f438</Sid>
		<FriendlyName>My Nice Twilio Account</FriendlyName>
		<Status>2</Status>
		<StatusText>Active</StatusText>
		<DateCreated>Wed, 02 Apr 2008 17:33:38 -0700</DateCreated>
		<DateUpdated>Wed, 02 Apr 2008 17:34:18 -0700</DateUpdated>
		<AuthToken>3a2630a909aadbf60266234756fb15a0</AuthToken>
	</Account>
</TwilioResponse>
~~~

##### PUT or POST

Allows you to modify the following properties of your Account:

* FriendlyName: The human-readable name given to your account.

> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438 HTTP/1.1
> FriendlyName=My+Nicer+Twilio+Account

~~~
<TwilioResponse>
	<Account>
		<Sid>AC309475e5fede1b49e100272a8640f438</Sid>
		<FriendlyName>My Nicer Twilio Account</FriendlyName>
		<Status>2</Status>
		<StatusText>Active</StatusText>
		<DateCreated>Wed, 02 Apr 2008 17:33:38 -0700</DateCreated>
		<DateUpdated>Wed, 02 Apr 2008 17:34:18 -0700</DateUpdated>
		<AuthToken>3a2630a909aadbf60266234756fb15a0</AuthToken>
	</Account>
</TwilioResponse>
~~~

##### DELETE

Not supported. You can't delete your account.




### Accounts List Resource

Right now, your credentials are tied to one and only one account, so you'll only get a list containing your single account. Not too useful, we know. If you're really curious you can make a GET request the the URI below. Or you can just ignore it.

#### Resource URI

> /2008-08-01/Accounts
