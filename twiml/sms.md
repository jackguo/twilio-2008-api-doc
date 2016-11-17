<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twml">latest version</a>.
</div>

# TwiML <span class="docs-tm">TM</span> Voice: Sms Verb

The `<Sms>` verb sends an SMS message to a phone number during a phone call.

### Verb Attributes {#attributes}

The `<Sms>` verb supports the following attributes that modify its behavior:

Attribute Name	|Allowed Values	|Default Value
:-------------	|:-------------	|:------------
[to](#to)	    |	phone number	|	see below
[from](#from)     |	phone number  | see below
[action](#action) 	|	relative or absolute URL	|	none
[method](#method) 	|	GET, POST	|	POST
[statusCallback](#statusCallback)	| 	relative or absolute URL | none

#### to {#to}

The 'to' attribute takes a valid phone number as a value. Twilio will send an
SMS message to this number. If no 'to' attribute is provided, Twilio will pick a default.
When sending an SMS on an incoming call, 'to' defaults to the caller. When sending an SMS on an outgoing call, 'to' 
defaults to the called party. The value of 'to' must be a valid phone number. NOTE: sending to short codes is not currently supported.  

<div class="alert alert-error">
Note: If your account is a Free Trial account, the provided 'to' phone number must be validated with Twilio.
However you can send an SMS to any caller if you don't explicitly specify a 'to' number.
</div>

#### from {#from}

The 'from' attribute takes a valid phone number as an argument. This number must be a phone number that you've
purchased from Twilio. If no 'from' attribute is provided, Twilio will pick a default. When sending an SMS on 
an incoming call, 'from' defaults to the called party. When sending an SMS on an outgoing call, 'from' defaults to the calling party.
This number must be an SMS-capable local phone number assigned to your account. If the phone number isn't SMS-capable,
then the `<Sms>` verb will not send an SMS message.

#### action {#action}

The 'action' attribute takes a URL as an argument. After processing the `<Sms>` verb,
Twilio will make a GET or POST request to this URL with the form parameters "SmsStatus" and "SmsSid". Using an 'action' URL,
your application can receive synchronous notification that the message was successfully enqueued.
If no action is provided, Twilio will unconditionally move on to the next verb in the document. If there
is no next verb, Twilio will end the phone call. Note that this is different from the
behavior of [`<Record>`](record) and [`<Gather>`](gather). `<Sms>` does not submit a request back to the current document URL by default if no 'action' is provided. Instead, it continues
on to execute the next TwiML verb.

"action URL request parameters"
Parameter | Description
:-------- | :----------
SmsSid       | The Sid for the Sms message
SmsStatus | The current status of the SMS message

"possible SmsStatus values"
Value  	  | Description
:-------- | :----------
sending	  | The message is in the process of being sent
invalid	  | Twilio was unable to deliver the message because one or more attributes you provided were not valid

#### method {#method}

The method attribute takes the value GET or POST. This tells Twilio
whether to request the action URL via HTTP GET or POST. This attribute
is modeled after the HTML form method attribute. POST is the default value.

#### statusCallback {#statusCallback}

The statusCallback attribute takes a URL as an argument. When the SMS message is actually sent, or if sending fails,
Twilio will make a POST request to this URL with the form parameters 'SmsStatus' and 'SmsSid'.  Note, statusCallback always uses HTTP POST to request the given url.

"statusCallback URL request parameters"
Parameter | Description
:-------- | :----------
SmsSid       | The Sid for the Sms message
SmsStatus | The current status of the Sms message. Sending, Failed, Received, Sent

"possible SmsStatus values"
Value  	  | Description
:-------- | :----------
sending	  | The message is in the process of being sent
invalid	  | Twilio was unable to deliver the message because one or more attributes you provided were not valid

### Nouns {#nouns}

The "Noun" of a Twilio verb is the text body of the XML element, the thing the verb
acts upon. In the case of `<Sms>`, the noun is the text of the SMS you wish to send.

### Nesting Rules {#nesting-rules}

The `<Sms>` verb can be nested in the following elements:

* [`<Response>`](response)

The following verbs can be nested within `<Sms>`:

* none

### See Also {#see-also}

[Twilio REST API SMS Messages Resource](/docs/api/rest/sms) - Using the REST API to send SMS messages and retrieve logs

### Examples {#examples}

#### Example 1: Simple Sms use case {#examples-1}

~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
	<Say>Our store is located at 123 Easy St.</Say>
	<Sms>Store Location: 123 Easy St.</Sms>
</Response>    
~~~

This is the simplest case for `<Sms>`.  Twilio first tells the caller
where the store is located, and then sends the caller an SMS with the 
location as the message.

#### Example 2: SmsStatus reporting {#examples-2}
~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
	<Say>Our store is located at 123 Easy St.</Say>
    <Sms action="/smsHandler.php" method="POST">Store Location: 123 Easy St.</Sms>
</Response>
~~~

In this use case, we have provided an action url and method.  Now when the message is queued for delivery, 
Twilio will submit to the action URL with the parameter 'SmsStatus'.  If the messages is queued and waiting
 to be sent, SmsStatus=sending.  If an invalid attribute was provided, then SmsStatus=invalid.  

Your web application can look at the SmsStatus parameter and decide what to 
do next.   

If an action URL is provided for `<Sms>`, flow of your application will continue at that URL.  
All verbs remaining in the document will be unreachable and ignored.

#### Example 3: statusCallback reporting {#examples-3}

~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
	<Say>Our store is located at 123 Easy St.</Say>
    <Sms statusCallback="/smsHandler.php">Store Location: 123 Easy St.</Sms>
</Response>
~~~

In this use case, we have provided a statusCallback url.
When the message is finished sending (not just enqueued), Twilio will asynchronously submit to the action URL 
with the parameter SmsStatus.  If the messages was successfully sent, SmsStatus=sent.  If the message failed to send, 
SmsStatus=failed.



