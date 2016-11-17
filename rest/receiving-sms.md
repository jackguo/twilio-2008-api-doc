<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# Receiving Inbound SMS Messages
Your SMS-enabled incoming phone numbers will have a callback-url specified, so
that if somebody SMSes your phone number, Twilio will consult your URL with the
Sid, From, To, and Body of the message. You can easily respond to the SMS by
returning a text/plain response to Twilio.

### Twilio's Request to your Application
When an SMS is received by Twilio, we will perform an HTTP request to your webserver.
The following fields will be sent (via POST, or via URL query parameters, depending on
which HTTP method you've chosen):

"SMS Request"
Property  | Description
:-------------	|:-------------
SmsMessageSid	|A 34 character unique identifier for the message. May be used to later retrieve this message from the REST API
AccountSid 	 | The 34 character id of the [Account][account] this Message is associated with. (Your account!) 
From            |The phone number that sent this message
To              |The phone number of the recipient
Body            |The text body of the SMS message. Up to 160 characters long.

### Your Response to Twilio
You may respond with a text/plain message of 160 characters or less, and the text body of
your response will be sent to the remote party. If you respond with an empty body (ie, no
response), then no message will be sent.

### Cookies
Twilio will keep cookie state across multiple SMS messages between the same two phone
numbers. This allows you to treat the separate messages as a conversation, and store data
about the conversation in the cookies for future reference. Twilio will expire the cookies for that conversation after four hours of inactivity.
