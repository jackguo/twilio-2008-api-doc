<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: Making Calls

Since initiating outbound calls via the REST API is such a frequent task, it get its own section.
  
### POST to the Calls URI 

To make a call, you perform an HTTP POST to your Calls list resource URI:

> /2008-08-01/Accounts/{YourAccountSid}/Calls 

##### POST Parameters: 

The following properties are used in your POST to initiate a phone call:
  
"Call POST Parameters"
 Param      | Optional | Description
 :---------- | :-------- | :------------- 
 Caller     | Required | A ten digit phone number to be used as caller id. It must be a valid outgoing caller id for your account, either a Twilio phone number or a Caller ID that you validated.
 Called     | Required | The number to call. Twilio will accept any format, such as (xxx) xxx-xxx, or xxx-xxx-xxxx. Pretty much anything. You may include a leading 1 or +1 for US phone numbers. Don't worry, we'll figure it out.  For calls outside the US, you must include the plus sign and country code.  For example: +44 for UK calls.
 Url        | Required | The fully qualified URL that should be consulted when the call connects. Just like when you set a URL for your inbound calls.
 Method     | Optional | The HTTP method Twilio should use when requesting the above URL. Defaults to POST.
 SendDigits | Optional | A string of keys to dial after connecting to the number. Valid digits in the string include: any digit (0-9), #, and *. For example, if you connected to a company phone number, and wanted to dial extension 1234 and then the pound key, use SendDigits="1234#". Remember to URL-encode this string, since the # character has special meaning in a URL. 
 IfMachine  | Optional | Tell Twilio to try and determine if a Machine (like voicemail) or a Human has answered the call. Possible values are: Continue or Hangup. See the [answering machines section][1] below for more info.
 Timeout    | Optional | The number of integer seconds that Twilio should allow the phone to ring before assuming there is no-answer. Default is 60 seconds, the maximum is 999 seconds. Note, you could set this to a low value, such as 15, to hangup before reaching an answering machine or voicemail. Also see the [answering machine section][1] for other solutions.

### Examples 

Making a call from 415-867-5309 to 415-555-1212, instructing Twilio to POST to http://www.myapp.com/myhandler.php to retrieve the TwiML for handling the call.

> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls HTTP/1.1 
> Caller=4158675309&Called=4155551212&Url=http://www.myapp.com/myhandler.php 

~~~
<TwilioResponse>
	<Call> 
		<Sid>CA42ed11f93dc08b952027ffbc406d0868</Sid> 
		<CallSegmentSid/> 
		<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid> 
		<Called>4155551212</Called> 
		<Caller>4158675309</Caller> 
		<PhoneNumberSid>PN01234567890123456789012345678900</PhoneNumberSid> 
		<Status>0</Status> 
		<StartTime>Thu, 03 Apr 2008 04:36:33 -0400</StartTime> 
		<EndTime/> 
		<Price/> 
		<Flags>1</Flags> 
	</Call> 
</TwilioResponse>
~~~

Making a call from 415-867-5309 to 415-555-1212, instructing Twilio to GET http://www.myapp.com/myhandler.php to retrieve the TwiML for handling the call.

> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls HTTP/1.1    
> Caller=415-867-5309&Called=4155551212&Url=http://www.myapp.com/myhandler.php&Method=GET 

~~~
<TwilioResponse>
	<Call>
		<Sid>CA42ed11f93dc08b952027ffbc406d0868</Sid> 
		<CallSegmentSid/> 
		<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid> 
		<Called>4155551212</Called> 
		<Caller>4158675309</Caller> 
		<PhoneNumberSid>PN01234567890123456789012345678900</PhoneNumberSid> 
		<Status>0</Status> 
		<StartTime>Thu, 03 Apr 2008 04:36:33 -0400</StartTime> 
		<EndTime/> 
		<Price/> 
		<Flags>1</Flags> 
	</Call> 
</TwilioResponse> 
~~~

Making a call from 866-867-5309 to 415-555-1212, after connecting dialing extension 1234#, instructing Twilio to GET http://www.myapp.com/myhandler.php to retrieve the TwiML for handling the call.

> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Calls HTTP/1.1
> Caller=866-867-5309&Called=4155551212&Url=http://www.myapp.com/myhandler.php&Method=GET&SendDigits=1234%23 

~~~
    <TwilioResponse>  
    	<Call>  
    		<Sid>CA42ed11f93dc08b952027ffbc406d0868</Sid>  
    		<CallSegmentSid/>  
    		<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid>  
    		<Called>4155551212</Called>  
    		<Caller>8668675309</Caller>  
			<PhoneNumberSid>PN01234567890123456789012345678999</PhoneNumberSid> 
    		<Status>0</Status>  
    		<StartTime>Thu, 03 Apr 2008 04:36:33 -0400</StartTime>  
    		<EndTime/>  
    		<Price/>  
    		<Flags>1</Flags>  
    	</Call> 
    </TwilioResponse>
~~~

### Handling Possible Call Outcomes

When the call begins, Twilio will request the provided URL to receive call instructions in TwiML. At this point, your interaction with Twilio via HTTP is identical to that of an inbound call. There is one exception: 

#### DialStatus 

Twilio sends a DialStatus parameter to your application, indicating the result of your new call. This parameter will be appended to the URL for a GET request, or added to the POST fields. Possible values are:

  
*   **answered:** The call has been answered and IfMachine was not specified. (See the [answering machines][1] section below for more info)
*   **answered-human:** The call has been answered by a human and IfMachine was Hangup or Continue. (See the [answering machines][1] section below for more info)
*   **answered-machine:** The call has been answered by a machine and IfMachine was Continue. (See the [answering machines][1] section below for more info)
*   **hangup-machine:** The call has been answered by a machine and IfMachine was Hangup. (See the [answering machines][1] section below for more info)
*   **busy:** The phone number was busy, and was not answered. No retries will be made.
*   **no-answer:** The phone number was not picked up after ringing for Timeout seconds (defaults to 60 seconds). No retries will be made.
*   **fail:** An intermittent error has occurred, and Twilio was unable to complete the call. No retries will be made.

#### Answering Machines {#answering-machine}

Twilio can try to detect if an answering machine has answered a call, and can handle the call differently if it is indeed a machine. By default, Twilio does not attempt to determine if a machine or a human has answered the call... it just commences with the call flow.
  
<div class="alert alert-error">
IMPORTANT: The downside of detecting a human vs. a machine is that Twilio needs to listen to the first few seconds of audio after picking up the call. This may result in a few seconds delay before the call begins. If your application does not care about the human vs. machine distinction, then omit the "IfMachine" parameter and Twilio will perform no such analysis. 
</div>

##### Default Behavior 

By default, Twilio does not try and detect if the answering party is a machine (or voicemail), or a human. If anybody or anything picks up the call, Twilio will begin executing the call flow immediately, and will notify your application URL by sending a DialStatus=answered. Note, if you're expecting to "leave a message" on an answering machine, the call flow will likely begin before the "beep" is reached, and therefore the beginning of your call content will not get captured by the machine. See IfMachine=Continue below.
  
##### Hanging Up 

If A Machine Answers If your application does not want to talk to answering machines, then you can specify IfMachine=Hangup when you start the call.
  
If Twilio detects that a machine, not a human, has answered the call, Twilio will immediately hangup. Twilio will notify your application URL by sending a DialStatus=hangup-machine. If you encounter this DialStatus, your response to the request doesn't matter... we're just notifying you that the call went through and a machine answered.  
  
If Twilio detects a human has answered the call, then Twilio will make a request to your application URL with DialStatus=answered-human and the call flow will proceed as normal. 
 
##### Continuing If A Machine Answers 

If your application would like to know if a human or a machine answered, and will perhaps return different content based on the situation, then you can specify IfMachine=continue when you start the call.

If Twilio detects that a machine, not a human, has answered the call, Twilio will make a request to your application URL with DialStatus=answered-machine. The call flow will proceed as normal, and your application can choose to customize the content of the call for a recorded greeting. Twilio will wait until the familiar "BEEP" of an answering machine to begin executing your call flow, so using a `<Play>` or `<Say>` will be captured by the answering machine (or voicemail). Keep in mind, that if a machine answers, you'll want to avoid using `<Gather>` or `<Record>`, which require user input.  
  
If Twilio detects that a human has answered the call, Twilio will make a request to your application URL with DialStatus=anwered-human and call flow will proceed as normal.  

##### What to do if you get a Human? 

Go ahead with your call, mmmkay? 

##### What to do if you get a Machine? 

If you do want to leave a message on a machine, then initiate the call with IfMachine=Continue and check the DialStatus parameter when Twilio makes a request to your server.  If DialStatus is "answered-machine", then `<Say>` or `<Play>` the content of your message. Twilio will automatically play the message after the answering machine beep.

If you don't want to leave a message on a machine, then you should initiate the call with IfMachine=Hangup. Since Twilio waits for the machine's BEEP before consulting your application, if you were to hangup at this point, you'd probably end up leaving a second or two of silence in a voicemail message... which isn't very nice. IfMachine=hangup solves this problem by first hanging up the call, and then notifying your application.

[1]: #answering-machine
