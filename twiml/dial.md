<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# TwiML <span class="docs-tm">TM</span> Voice: Dial Verb

The `<Dial>` verb connects the current caller to an another phone. If the called party 
picks up, the two parties are connected and can communicate until one hangs up. If the 
called party does not pick up, if a busy signal is received, or if the number doesn't 
exist the dial verb will finish.

### Verb Attributes {#attributes}

The `<Dial>` verb supports the following attributes that modify its behavior:

Attribute Name	|Allowed Values	|Default Value
:-------------	|:-------------	|:------------
[action](#action) 	|	relative or absolute URL	|	no default action for Dial
[method](#method) 	|	GET, POST	|	POST
[timeout](#timeout)	|	positive integer	|	30 seconds
[hangupOnStar](#hangup) |	true, false  | false
[timeLimit](#limit)	| 	positive integer (seconds) | 	14400 seconds (4 hours)
[callerId](#caller) | 	valid phone number |	Caller's callerId

#### action {#action}

The action attribute takes a URL as an argument.  When the remote party hangs up,
Twilio will make a GET or POST request to this URL with the form parameter "DialStatus".
If no action is provided, `<Dial>` will finish and Twilio will move on to the next verb
in the document.  If there is no next verb, twilio will end the phone call.
Note that this is different from the behavior of `<Record>` and `<Gather>`. `<Dial>` does not 
submit a request to the current document's URL by default if no action URL is provided.

"action URL request parameters"
Parameter | Description
:-------- | :----------
DialStatus | The outcome of the `<Dial>` attempt

"Possible DialStatus Values"
Value  	  | Description
:-------- | :----------
answered  |	The called party answered the call and was connected to the caller
busy 	  | Twilio received a busy signal when trying to connect to the called party
no-answer |	The called party did not pick up before the timeout period passed
failed 	  | Twilio was unable to route to this phone number. This is frequently caused by dialing a properly formatted, but non-existent phone number. 


#### method {#method}

The method attribute takes the value GET or POST.  This tells Twilio 
whether to submit the action URL as a GET or POST method.  This attribute
is modeled after the HTML form method attribute. POST is the default value.
		
#### timeout {#timeout}

The timeout attribute sets the limit in seconds that `<Dial>` waits for the called party to 
answer the call.  Basically, how long should Twilio let the call ring before giving up
and reporting "no-answer" as the DialStatus.

#### hangupOnStar {#hangup}

The hangupOnStar attribute lets the calling party hang up on the called party by
pressing the '*' key on his phone.  When two parties are connected using 
Dial, Twilio blocks execution of further verbs until the caller or called parties 
hang up.  This feature allows the calling party to hang up on the called party without 
having to hang up her phone and ending her Twilio session.  When the caller presses '*' 
Twilio will hang up on the called party.  If an action URL was provided, Twilio submits
the 'answered' dial status to the URL and processes the response.  If no action was provided
Twilio will continue on to the next verb in the current document.  

#### timeLimit  {#limit}

The timeLimit attribute sets the maximum duration of the `<Dial>` in seconds.  For example,
by setting a time limit of 120 seconds `<Dial>` will hang up on the called party 
automatically 2 minutes into the phone call.  By default, there is a four hour time limit set on calls. 

#### callerId {#caller}

The callerId attribute lets you change the caller ID that appears 
to the called party when you dial.  By default, this is the caller ID of
the calling party.

For example:  An inbound caller to Twilio, with the caller id 415-123-4567, 
executes a `<Dial>` verb to call 858-987-6543.  The called party will see 
415-123-4567 as the caller ID.  

You are allowed to change the phone number that the called party sees to one 
of the following:

* the caller ID of the incoming phone call 
* any inbound phone number you have purchased from Twilio 
* any phone number you have verified with twilio for use as a caller ID



### Nouns {#nouns}

The "Noun" of a Twilio verb is the body of the element, the thing the verb acts 
upon.  In the case of Dial, the noun is the information that tells Twilio what 
telephony device(s) it should try to connect to. 

"Allowed &lt;Dial&gt; noun values"
type  	   | description
:-------   | :--------
String 	   | A string representing a valid phone number
`<Number>` | A nested XML element that describes a phone number with more complex attributes
`<Conference>` | A nested XML element that describes a conference allowing two or more parties to talk

#### `<Number>` Noun {#number}

The number element specifies a phone number.
The number element has two optional attributes: <i>sendDigits</i> and <i>url</i>.

The <i>sendDigits</i> attribute tells Twilio to play DTMF tones when 
the phone answers.  An example of where this is useful is 
dialing a persons phone number and extension.  Twilio will dial the 
main phone number, and when the automated system picks up, twilio will 
then send the DTMF digits for a persons extension, so that the remote 
system will then dial through to that person.

The <i>url</i> attribute allows you to specify a url for a TwiML document that 
will run on the called party's end, after they answer, but before the parties are connected.  
This TwiML doc can be used to privatly play or say information to the called party,
or offer them the chance to decline the phone call by issuing a `<Hangup/>` verb. 
The calling party will continue to hear ringing while the TwiML document 
executes on the other end.  TwiML documents executed in this manner are not 
allowed to contain the `<Dial>` verb.

The Number element also allows you to specify several phone numbers to dial 
within a single `<Dial>` verb.  When `<Dial>` has more than one `<Number>` 
element, it dials them all simultaneously.  The first person to pick up gets connected
and the rest are hung up on.

#### `<Conference>` Noun {#conference}

The conference noun allows you to `<Dial>` into a conference room, rather than `<Dial>` another number.
See the documentation on the [`<Conference`>](conference) noun for a detailed walkthrough of how to use Twilio's conferencing functionality.

### Nesting Rules {#nesting}

The `<Dial>` verb can be nested in the following elements:

* [`<Response>`](response)

The following elements can be nested within `<Dial>`:

* [`<Number>`](#number)
* [`<Conference>`](#conference)

### See Also {#see-also}
[Twilio REST Call object](/docs/api/rest/call) - Initiating outbound phone calls

### Examples {#examples}

#### Example 1: Simple Dial use case {#example-1}

~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
	<Dial>415-123-4567</Dial>
	<Say>Goodbye</Say>
</Response>    
~~~

This is the simplest case for Dial.  Twilio will dial 415-123-4567. 
If someone answers, Twilio will connect the caller to the called party. 
If the caller hangs up, the Twilio session ends.  If the line is busy, 
if there is no answer, or if the called party hangs up, 
`<Dial>` exits and the `<Say>` verb is
executed for the caller before the twilio session ends.

#### Example 2: DialStatus reporting {#example-2}
~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
	<Dial action="/handleDialStatus.php" method="GET">
		415-123-4567
	</Dial>
	<Say>I am unreachable</Say>
</Response>
~~~

In this use case, we have provided an action url and method.  
Now when `<Dial>` ends, Twilio will submit to the action URL with the parameter
DialStatus.  If nobody picks up, DialStatus=no-answer.  If the line is busy, 
DialStatus=busy.  If the called party picked up, DialStatus=answered.  If an 
invalid phone number was provided, DialStatus=failed.  

Your web application can look at the DialStatus parameter and decide what to 
do next.   

If an action URL is provided for Dial, Twilio will always post to it, regardless of outcome of Dial. 
All verbs remaining in the document will be unreachable and ignored. 

#### Example 3: Number element and sendDigits {#example-3}

~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
	<Dial>
		<Number sendDigits="wwww1928">
			415-123-4567
		</Number>
	</Dial>
</Response>
~~~

In this case, we want to dial the 1928 extension at 415-123-4567. 
We use the nested `<Number>` tag to describe the phone number noun, and 
give it the attribute sendDigits.  Send digits contains the extension 1928, 
as well as four leading 'w' characters.  The 'w' characters tell Twilio 
to wait 0.5 seconds instead of playing a digit.  This lets you adjust the
timing of when the digits begin playing, depending on the nature of the 
phone system you are dialing.

#### Example 4: Simultaneous Dialing  {#example-4}

~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
	<Dial>
		<Number>
			858-987-6543
		</Number>
		<Number>
			415-123-4567
		</Number>
		<Number>
			619-765-4321
		</Number>
	</Dial>
</Response>
~~~

In this case we use several different Number tags to dial 3 phones at the same time. 
The first of these phones to answer will be connected to the caller, the rest of the 
connection attempts will be canceled.  

### Hints and Advanced Uses {#hints}

*   Simultaneous dial can be useful when you have several phones (or several people) 
	that you want to ring when an incoming call comes in.  Keep in mind that the first
	phone that picks up cancels all the other connections.  If you dial an office phone 
	system or a cellphone in airplane mode, it may pick up after a single ring,
	preventing the other phones from ringing long enough for a human to ever answer.
	
	You should take care to use simultaneous dial in situations where you know the behavior of 
	the called parties. 


