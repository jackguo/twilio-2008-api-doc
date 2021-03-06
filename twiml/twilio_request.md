<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# Receiving a Twilio Call Request

### How Twilio Passes Data to Your Application {#twilio-data-passing}

Twilio sends data to your application using either an HTTP POST or GET.

#### HTTP POST Variables {#http-post-variables}

When Twilio performs an HTTP POST, it passes metadata about the call as 
form field variables. The following variables are always POST'ed:

Property  | Description
:-------------	|:-------------
CallSid |  A unique identifier for this call, generated by Twilio. It's 34 characters long, and always starts with the letters CA.
Caller |  The phone number of the party that initiated the call. If the call is inbound, then it is the caller's caller-id. If the call is outbound, i.e., initiated by making a request to the REST Call API, then this is the phone number you specify as the caller-id.
Called |  The phone number of the party that was called. If the call is inbound, then it's your application phone number. If the call is outbound, then it's the phone number you provided to call.
AccountSid |  Your Twilio account number. It is 34 characters long, and always starts with the letters AC.
CallStatus |  The status of the phone call. The value can be "in-progress", "completed", "busy", "failed" or "no-answer". For a call that was answered and is currently going on, the status would be "in-progress". For a call that couldn't be started because the called party was busy, didn't pick up, or the number dialed wasn't valid: "busy", "no-answer", or "failed" would be returned. If the call finished because the call ended or was hung up, the status would be "completed".

Twilio also attempts to look up geographic data based on the caller and called phone numbers. The following fields are sent, if available:

Property  | Description
:-------------	|:-------------
CallerCity |  The city of the caller.
CallerState |  The state or province of the caller.
CallerZip |  The postal code of the caller.
CallerCountry |  The country of the caller.
CalledCity |  The city of the called party.
CalledState |  The state or province of the called party.
CalledZip |  The postal code of the called party.
CalledCountry |  The country of the called party.

Depending on the what is happening on a call, other variables may also be sent. The XML Verbs section goes into more detail.

#### The First Request {#the-first-request}

The first time Twilio makes a request to your document, it sends a parameter DialStatus. This value represents the result of the call attempt. For an outgoing call, the possible values are answered, busy, failed, or no-answer. DialStatus is always reported as "answered" for incoming calls, for the sake of consistency. If answering machine detection is being used, the additional values answered-machine, answered-human, and hangup-machine may be sent, depending on your settings. 

#### How Do I Know When The Call Ends? {#call-ends}

When all parties hang up and the call ends, Twilio will notify your system of this event by making an additional HTTP request to your initial URL. It will send the parameter "CallStatus=completed" as part of this request If your system needs to capture this "end of call" event, you can put logic in the script for your initial URL that watches for the CallStatus=completed case.

#### HTTP GET Variables {#http-get-variables}

When Twilio performs an HTTP GET, it passes metadata about the call as URL query string parameters. The names and behavior are the same as those values passed in a POST. Here's an example query string with the above parameters:

> http://myserver/callhandler.php?CallSid=CA39485958d938b&Caller=4158675309&Called=4155551212&AccountSid=AC394848B8393DF323 

