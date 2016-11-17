<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# TwiML <span class="docs-tm">TM</span> Voice: Record Verb

The `<Record>` verb records the caller's voice and returns to you the URL of a file containing the audio recording. You can optionally generate text transcriptions of recorded calls by setting the 'transcribe' attribute of the `<Record>` verb to "true."

### Verb Attributes {#attributes}

The `<Record>` verb supports the following attributes that modify its behavior:

Attribute Name	|Allowed Values	|Default Value
:-------------	|:-------------	|:------------
[action](#action) 	|	relative or absolute URL	|	current document url
[method](#method) 	|	GET, POST	|	POST
[timeout](#timeout)	|	positive integer	|	5 seconds
[finishOnKey](#finishonkey)	|	any digit, #, *	|	1234567890*#
[maxLength](#maxlength)	| 	integer greater than 1	|	3600 (1 hour)
[transcribe](#transcribe)	|	true, false 	|	false
[transcribeCallback](#callback)	| 	relative or absolute URL	|	none
[playBeep](#playBeep)	|	true, false | true

#### action {#action}

The action attribute takes an absolute or relative URL as a value. When recording is finished Twilio will make a GET or POST request to this URL with the form parameters "RecordingUrl", "Duration" and "Digits". If no action is provided, `<Record>` will default to requesting the current document's URL.

"action URL request parameters"
Parameter | 	Description
:-------------	|:-------------
RecordingUrl | the URL of the recorded audio
Duration | the time duration of the recorded audio
Digits		| the key (if any) pressed to end the recording

#### method {#method}

The method attribute takes the value GET or POST. This tells Twilio whether to submit the action URL as a GET or POST method. This attribute is modeled after the HTML form method attribute. POST is the default value.

#### timeout {#timeout}

The timeout attribute tells Twilio to end the recording after this many seconds of silence have passed. The default is 5 seconds.

#### finishOnKey {#finishonkey}

The finishOnKey attribute lets you choose a set of digits that end the recording. For example, if you set finishOnKey="#", and the user presses #, Twilio will immediately stop recording and submit the RecordingUrl, Duration, and the '#' to the action URL. The allowed values are the numbers 0-9, # and *. The default is '1234567890*#' (any key will end the recording). Unlike `<Gather>`, you may specify more than one character as a finishOnKey value.

#### maxLength {#maxlength}

The maxLength attribute lets you set the maximum length of the recording in seconds. If you set maxLength="30", the recording will automatically end and submit after 30 seconds of recording. Defaults to 3600 seconds (1 hour) for a normal recording and 120 seconds (2 minutes) for a transcribed recording.

#### transcribe {#transcribe}

The transcribe attribute tells Twilio that you would like a text representation of the audio of the recording. Twilio will pass this recording to our speech-to-text engine and attempt to convert the audio to human readable text.
The transcribe option is off by default. If you do not wish to perform transcription, do not include the transcribe attribute.

<div class="alert alert-error">
Note: transcription is a pay feature. If you include a transcribe or transcribeCallback attribute in your Record verb, your account will be charged. See the <a href="http://www.twilio.com/pricing-signup">pricing</a> page for our transcription prices.
<br/><br/>
Additionally, transcription is currently limited to recordings with a duration of 2 minutes or less. If you enable transcription and set maxLength > 120 seconds, Twilio will write a warning to your debug log rather than transcribing the recording.
</div>

#### transcribeCallback {#callback}

The transcribeCallback attribute is used in conjunction with the "transcribe" attribute. It allows you to specify a URL to which Twilio will make a POST request when the transcription is complete. The request Twilio sends will contain the standard Twilio parameters (AccountSid, CallSid, Caller, etc.) as well as "TranscriptionStatus", "TranscriptionText", "TranscriptionUrl" and "RecordingUrl". If transcribeCallback is not specified, the completed transcription will be stored for you to retrieve later (see the REST API [Transcriptions](/docs/api/rest/transcription) section), but Twilio will not asynchronously notify your application.

"transcribeCallback URL request parameters"
Parameter | 	Description
:-------------	|:-------------
TranscriptionText |	contains the text of the transcription
TranscriptionStatus |	status of the transcription attempt: "completed" or "failed"
TranscriptionUrl |	a URL representing the transcription REST resource
RecordingUrl |	a URL representing the transcription's source recording resource

#### playBeep {#playBeep}

The playBeep attribute allows you to toggle between playing a sound before the start of a recording.  If you set the value to false, no beep sound will be played.

### Nouns {#nouns}

The `<Record>` verb has no "noun". It does not act on anything.

### Nesting Rules {#nesting-rules}

The `<Record>` verb can be nested in the following elements:

* [`<Response>`](response)

The following verbs can be nested within `<Record>`:

* none

### See Also {#see-also}

[Twilio REST API Recording resource](/docs/api/rest/recording)

[Twilio REST API Transcription resource](/docs/api/rest/transcription)

### Examples {#examples}

#### Example 1: default behaviour {#examplse-1}

~~~
<?xml version="1.0" encoding="UTF-8"?>  
<Response>  
	<Record/>  
</Response>     
~~~

Twilio will execute the `<Record>` verb causing the caller to hear a beep and the recording to start.  If the caller is silent for more than 5 seconds, hits the # key, or the recording maxlength time is hit, Twilio will make an HTTP POST request to the default action (the current document URL) with the parameters RecordingUrl and Duration. 

#### Example 2: simple recording prompt {#examples-2}
~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
	<Say>
		Please leave a message at the beep. 
		Press the star key when finished. 
	</Say>
	<Record 
		action="http://foo.edu/handleRecording.php"
		method="GET" 
		maxLength="20"
		finishOnKey="*"
		/>
	<Say>I did not receive a recording</Say>
</Response>
~~~

This is example shows a simple voicemail prompt.  The caller is asked to leave a message at the beep.  The `<Record>` verb beeps and begins recording up to 20 seconds of audio.

* If the caller does not speak at all, the Record verb exits after 5 seconds of silence,  falling through to the next verb in the document. In this case, it would fall through to the &lt;Say&gt; verb. 
* If the caller speaks for less that 20 seconds and is then silent for 5 seconds, Twilio makes a GET request to the action URL.  The Say verb is never reached.
* If the caller speaks for the full 20 seconds, Twilio makes a GET request to the action URL.  The Say verb is never reached.

#### Example 3: transcription {#examples-3}

~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
	<Record transcribe="true" transcribeCallback="/handle_transcribe.php"/>
</Response> 
~~~

Twilio will record the caller. When the recording is complete, Twilio will transcribe the recording and make an HTTP POST request to the transcribeCallback URL containing a transcription of the recording.

### Hints and Advanced Uses {#hints}

* Twilio will trim leading and trailing silence from your audio files. This may cause the duration of the files to be slightly smaller than the time a caller spends recording them.
* If Twilio receives an empty recording, it will not submit, but will instead fall through to the next verb in the document. If there are no more verbs, the call ends.
