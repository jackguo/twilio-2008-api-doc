<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: SMS Messages

The SMS Messages list resource represents the set of SMS messages sent from and received by an account. An SMS Message instance resource represents an inbound or outbound SMS message.  When someone sends a text message from or to your application, either via the REST API or during a call via the `<Sms>` verb, Twilio creates an SMS Message instance resource and associates it with your account.




### SMS Message Instance Resource

This resource represents an individual SMS message.

#### Resource URI

> /2008-08-01/Accounts/{AccountSid}/SMS/Messages/{SMSMessageSid}

#### Resource Properties

An SMS Message resource is represented by the following properties:

"SMS Resource Properties"
Property  | Description
:-------------	|:-------------
Sid 			|A 34 character string that uniquely identifies this resource.
DateCreated 	|The date that this resource was created, given in RFC 2822 format.
DateUpdated 	|The date that this resource was last updated, given in RFC 2822 format.
DateSent	    |The date that the SMS was sent, given in RFC 2822 format.
AccountSid 	 | The 34 character id of the [Account][account] this Call is associated with. (Your account!)
From            |The phone number that initiated the message. For incoming messages, this will be the remote phone. For outgoing messages, this will be one of your Twilio phone numbers.
To              |The phone number that received the message. For incoming messages, this will be one of your Twilio phone numbers. For outgoing messages, this will be the remote phone. 
Body            |The text body of the SMS message. Up to 160 characters long.
Status          |Either queued, sending, sent, or failed.
Flags           |A bitwise field for storing extra information about a message. Possible values are: 1 means the message was inbound, 2 means the call was a reply to an incoming message, 4 means the SMS was outgoing via the REST API. These values may be OR'd together in the future, so be sure to check values with a bitwise operator.

[account]: account

#### HTTP Methods

##### GET

Returns a single SMS message specified by the provided Sid. Example: 

>GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/SMS/Messages/SM872fb94e3b358913777cdb313f25b46f HTTP/1.1

~~~
<TwilioResponse>
    <SMSMessage>
        <Sid>SM872fb94e3b358913777cdb313f25b46f</Sid>
        <DateCreated>Sun, 04 Oct 2009 03:48:08 -0700</DateCreated>
        <DateUpdated>Sun, 04 Oct 2009 03:48:10 -0700</DateUpdated>
        <DateSent>Sun, 04 Oct 2009 03:48:10 -0700</DateSent>
        <AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
        <To>4158675309</To>
        <From>6505551212</From>
        <Body>Hi there Jenny! Why didn't you call me back?</Body>
        <Status>sent</Status>
        <Flags>2</Flags>
    </SMSMessage>
</TwilioResponse>
~~~

##### POST

Not supported.

##### PUT

Not supported.

##### DELETE

Not supported.


### SMS Messages List Resource

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/SMS/Messages

#### HTTP Methods

##### GET

Returns a list of SMS messages made by your account. Each SMS is represented by an
`<SMSMessage>` element, under an `<SMSMessages>` list element that includes <a href='/docs/api/2008-08-01/rest/response#paging'>paging information</a>. The list is sorted by DateSent, with newest messages first. Example:

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/SMS/Messages HTTP/1.1 

~~~
<TwilioResponse>
    <SMSMessages page="0" numpages="2" pagesize="50" total="69" start="0"
end="49">
        <SMSMessage>
            <Sid>SM872fb94e3b358913777cdb313f25b46f</Sid>
            <DateCreated>Sun, 04 Oct 2009 03:48:08 -0700</DateCreated>
            <DateUpdated>Sun, 04 Oct 2009 03:48:10 -0700</DateUpdated>
            <DateSent>Sun, 04 Oct 2009 03:48:10 -0700</DateSent>
            <AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
            <To>4158675309</To>
            <From>6505551212</From>
            <Body>Hi there Jenny! Want to grab dinner?</Body>
            <Status>sent</Status>
            <Flags>2</Flags>
        </SMSMessage>
        <SMSMessage>
            <Sid>SM47dfd824add482e1fee25ee3ce216113</Sid>
            <DateCreated>Sun, 04 Oct 2009 03:48:07 -0700</DateCreated>
            <DateUpdated>Sun, 04 Oct 2009 03:48:07 -0700</DateUpdated>
            <DateSent>Sun, 04 Oct 2009 03:48:07 -0700</DateSent>
            <AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
            <To>6505551212</To>
            <From>4158675309</From>
            <Body>The judge said you can't text me anymore.</Body>
            <Status>sent</Status>
            <Flags>1</Flags>
        </SMSMessage>
    </SMSMessages>
</TwilioResponse>
~~~

##### URL Filtering 

You may limit the list by providing certain query string parameters to the listing resource. Note, parameters are case-sensitive:

* **To:** Only show SMS messages to this phone number.
* **From:** Only show SMS messages from this phone number.
* **DateSent:** Only show SMS messages sent on this date, given as YYYY-MM-DD. Example: DateSent=2009-07-06. You can also specify inequality, such as DateSent<=YYYY-MM-DD for SMS messages that were sent on or before midnight on a date, and DateSent>=YYYY-MM-DD for SMS messages that were sent on or after midnight on a date.

##### POST

Sends a new SMS message. For full details see the [Sending SMS](/docs/api/2008-08-01/rest/sending-sms) section.

"SMS Post Parameters"
Parameter  | Optional? | Description
:-------------	|:-------------  |:-------------
From            |Required |The phone number from which you'd like to send the message. Must be one of your Twilio phone numbers that is enabled for SMS.
To              |Required |The phone number to whom you'd like to send the SMS. May be any 10 digit North American number, or any international phone number.
Body            |Required |The text body of the SMS message. Up to 160 characters long.
StatusCallback  | Optional | A URL that Twilio will POST to when your message is processed.  An HTTP POST request will be made to this URL when the message is processed, posting the SmsSid as well as SmsStatus=sent or SmsStatus=failed.

For example:

> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/SMS/Messages HTTP/1.1
  To=4158675309&From=6505551212&Body=Hi+Jenny+u+didnt+return+my+last+text+whats+up

##### PUT

Not supported.

##### DELETE

Not supported.
