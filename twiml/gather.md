<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# TwiML <span class="docs-tm">TM</span> Voice: Gather Verb

The `<Gather>` verb collects digits that a caller enters into their telephone
keypad. When the caller is done entering data, Twilio submits that data to a provided URL
as either a HTTP GET or POST request, just like a web browser submits data from an HTML form.

If no input is received before timeout, `<Gather>` falls through to the next verb
in the TwiML document.

You may optionally nest [`<Say>`][say] and [`<Play>`][play]
verbs within a `<Gather>` verb while waiting for input. This allows you to read menu
options to the caller while letting her enter a menu selection at any time. After the first
digit is received the audio will stop playing.

[say]: say
[play]: play

### Verb Attributes {#attributes}

The `<Gather>` verb supports the following attributes that modify its behavior:

Attribute Name	|Allowed Values	|Default Value
:-------------	|:-------------	|:------------
[action](#action) 	|	relative or absolute URL  |	current document URL
[method](#method) 	|	GET, POST	|	POST
[timeout](#timeout)	|	positive integer	|	5 seconds
[finishOnKey ](#finish) |	any digit, #, * and the empty string  | #
[numDigits ](#digits) |  integer >= 1 |	unlimited

#### action {#action}

The action attribute takes an absolute or relative URL as a value.
When a caller completes entering data, Twilio will make a GET or POST
request to this URL with the form parameter "Digits". If no action attribute is
provided, the default value is the URL of the of the current document.

"action URL request parameters"
Parameter | Description
:-------- | :----------
Digits    | The digits received from the caller

#### method {#method}

The method attribute takes the value GET or POST. This tells Twilio
whether to request the action URL via HTTP GET or POST. This attribute
is modeled after the HTML form method attribute. POST is the default value.

#### timeout {#timeout}

The timeout attribute sets the limit in seconds that `<Gather>` will wait
for the caller to press another digit. For example, if timeout="10", Twilio
will wait 10 seconds for the caller to press another key before submitting
the previously entered digits to the action URL.

#### finishOnKey {#finish}

The finishOnKey attribute lets you choose one value that submits the
received data when entered. For example, if you set finishOnKey="#" and the user
enters 1234#, Twilio will immediately stop waiting for more input when
the # is received and will submit Digits=1234 to the action URL. Note that
the finishOnKey digit is not sent. The allowed values are the digits 0-9, #
, * and the empty string (finishOnKey=""). If the empty string is used, `<Gather>` captures
all input and no key will end the `<Gather>` when pressed. In this case Twilio will
submit the entered digits to the action URL only after the timeout has been reached.
The default is #. The value can only be a single character.

#### numDigits  {#digits}

The numDigits attribute lets you set the number of digits you are expecting,
and submits the data to the action URL when that number is reached. For example,
one might set numDigits="5" and ask the caller to enter their 5 digit zip code.
When the caller enters the fifth digit of 94117, Twilio will immediately submit
the data to the action URL.

### Nouns {#nouns}

`<Gather>` does not act upon any "Nouns"; the text body of the verb is always blank, although the `<Gather>` verb may contain a few other verbs.

### Nesting Rules  {#nesting-rules}

The `<Gather>` verb can be nested in the following elements:

* [`<Response>`][response]
    
[response]: response

The following elements can be nested within `<Gather>`:

* [`<Say>`][say]
* [`<Play>`][play]
* [`<Pause>`][pause]

[pause]: pause

### Examples {#examples}

#### Example 1: Default behavior of Gather {#examples-1}

~~~
<?xml version="1.0" encoding="UTF-8"?>
<!-- page located at http://example.com/simple_gather.xml -->
<Response>
	<Gather/>
</Response> 
~~~

This is the simplest case for a `<Gather>`.  When Twilio executes this verb
the application will pause for up to 5 seconds, waiting for the caller to 
enter digits on their keypad. 

* If they enter digits followed by a # symbol, Twilio submits those
  Digits as a POST back to the pages URL, http://example.com/simple_gather.xml
* If they enter digits followed by 5 seconds of silence, Twilio submits
  those Digits as a POST back to the pages URL, http://example.com/simple_gather.xml
* If they don't enter any digits and 5 seconds passes, Twilio does not
  submit to the URL, but instead moves on to the next verb in the file.  
  If there are no more verbs, Twilio hangs up

#### Example 2: Action/method and nested audio verb {#examples-2}
~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
	<Gather action="/process_gather.php" method="GET">
		<Say>
			Please enter your account number, 
			followed by the pound sign
		</Say>
	</Gather>
	<Say>We didn't receive any input. Goodbye!</Say>
</Response>
~~~

This is a more complex case.  First, we have added the action and method attributes. 
When digits are pressed on the keypad, they are submitted to the action URL.  
Second, we have added a nested [`<Say>`][say] verb.
This means that input can be gathered at any time during the `<Say>`.

*  If the caller enters a digit during the speaking of the text, the
[`<Say>`][say] verb will stop speaking and wait for digits, '#' sign, or a timeout.
*  If `<Gather>` tag times out without input, the [`<Say>`][say] verb
will complete and the `<Gather>` verb will exit without submitting. 
Twilio will then process the next verb in the document, which in this
case is a [`<Say>`][say] verb which informs the caller that
no input was received.
*  If the caller enters 12345 and then hits # or allows five seconds
to pass, Twilio will submit the digits as a GET request to
http://yourserver/process_gather.php?Digits=12345.

### Hints and Advanced Uses {#hints}

*   When there are nested [`<Say>`][say] and 
	[`<Play>`][play] verbs, the timeout begins after either the
	audio completes, or the first key is pressed.
*	When a `<Gather>` times out without user input, it will not submit to
	its action url, but will instead fall through to the next verb.  If you wish 
	to have the application submit on a timeout, use the [`<Redirect>`][redirect] verb: 
	
	~~~
	<?xml version="1.0" encoding="UTF-8"?>
	<Response>
		<Gather action="/process_gather.php" method="GET">
			<Say>Enter something, or not</Say>
		</Gather>
		<Redirect method="GET">
			/process_gather.php?Digits=TIMEOUT
		</Redirect>
	</Response>
	~~~
	
	When `<Gather>` times out, Twilio moves on to the next verb, in this
	case `<Redirect>`.  The `<Redirect>` verb instructs Twilio to make
	a new GET request to "/process_gather.php?Digits=TIMEOUT".
	
[redirect]: redirect

### Troubleshooting {#troubleshooting}

* 	**Problem:** `<Gather>` isn't receiving caller input when using a VoIP phone.  
	**Solution:** Some VoIP phones have trouble sending
	[DTMF](http://en.wikipedia.org/wiki/Dtmf) digits. This is usually because
	these phones use compressed, bandwidth conserving audio protocols that interfere
	with the transmission of the digit's signal.  Consult your phone's documentation
	on DTMF problems. 


