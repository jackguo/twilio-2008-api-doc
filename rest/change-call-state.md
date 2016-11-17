<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: Redirecting Live Calls

Realtime Call Redirection allows you to interrupt an in-progress call and have it begin 
processing TwiML from a new URL. This is useful for any application where you want to asynchronously 
change the behavior of a running call. For example: hold music, call queues, transferring calls, forcing hangup, etc.  

<div class="alert alert-error">
Note: Redirecting a call initiated by a Dial verb will end that call. The main call segment will continue on, but the second leg will be disconnected.
</div>

### POST to the Calls Instance Resource

> /2008-08-01/Accounts/{YourAccountSid}/Calls/{YourCallSid}

##### POST Parameters:

The following properties are used in your POST to redirect a phone call:

"Redirect POST Parameters"
Param      | Optional | Description
:---------- | :-------- | :------------- 
 CurrentUrl | Required | A valid URL that returns TwiML
 CurrentMethod | Optional | The HTTP method Twilio should use when requesting the above URL. Defaults to POST.

### Examples

Redirect a running call to a new URL

> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls/CA42ed11f93dc08b952027ffbc406d0868 HTTP/1.1
> CurrentUrl=http://www.myapp.com/myhandler.php&CurrentMethod=POST

~~~
<TwilioResponse>
	<Call> 
		<Sid>CA42ed11f93dc08b952027ffbc406d0868</Sid> 
		<CallSegmentSid/> 
		<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid> 
		<Called>4155551234</Called> 
		<Caller>4158675309</Caller> 
		<PhoneNumberSid>PN01234567890123456789012345678900</PhoneNumberSid> 
		<Status>1</Status> 
		<StartTime>Thu, 03 Apr 2008 04:36:33 -0400</StartTime> 
		<EndTime/> 
		<Price/> 
		<Flags>1</Flags> 
	</Call> 
</TwilioResponse>
~~~
