<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# Receiving a Twilio SMS Request

Your SMS-enabled incoming phone numbers will have a callback-url specified, so
that if somebody SMS your phone number, Twilio will consult your URL with the
Sid, From, To, and Body of the message. You can easily respond to the SMS by
returning a text/plain or TwiML response to Twilio.

### How Twilio Passes Data to Your Application {#twilio-data-passing}

Twilio sends data to your application using either an HTTP POST or GET.

#### HTTP POST Variables {#http-post-variables}

When an SMS is received by Twilio, we will perform an HTTP request to your webserver. The following fields will be sent (via POST, or via URL query parameters, depending on which HTTP method you've chosen):

"SMS Request"
Property  | Description
:-------------	|:-------------
SmsSid	|A 34 character unique identifier for the message. May be used to later retrieve this message from the REST API
AccountSid 	 | The 34 character id of the [Account][account] this Message is associated with. (Your account!) 
From            |The phone number that sent this message
To              |The phone number of the recipient
Body            |The text body of the SMS message. Up to 160 characters long.

Twilio also attempts to look up geographic data based on the caller and called phone numbers. The following fields are sent, if available:

Property  | Description
:-------------	|:-------------
FromCity |  The city of the sender
FromState |  The state or province of the sender.
FromZip |  The postal code of the called sender.
FromCountry |  The country of the called sender.
ToCity |  The city of the recipient.
ToState |  The state or province of the recipient.
ToZip |  The postal code of the recipient.
ToCountry |  The country of the recipient.

#### HTTP GET Variables {#http-get-variables}

If you use HTTP GET instead of POST, Twilio passes metadata about the call as URL query string parameters. The names and behavior are the same as those values passed in a POST. Here's an example query string with the above parameters:

> http://myserver/callhandler.php?Body=How+are+you+doing&From=4158675309&To=4155551212&AccountSid=AC394848B8393DF323 

