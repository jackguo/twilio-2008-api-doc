<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# TwiML <span class="docs-tm">TM</span> Voice: Play Verb

The `<Play>` verb plays an audio file back to the caller. Twilio retrieves the file from a URL that you provide.

### Verb Attributes {#attributes}

The `<Play>` verb supports the following attributes that modify its behavior:

Attribute Name	|Allowed Values	|Default Value
:-------------	|:-------------	|:------------
[loop](#loop) 	|	integer >= 0	|	1

#### loop {#loop}

The loop attribute specifies how many times the audio file is played. 
The default behavior is to play the audio once. 
Specifying 0 will cause the the `<Play>` verb to loop until the call is hung up. 


### Nouns {#nouns}

The "noun" of a Twilio verb is the text body of the element: the thing the verb
acts upon. In the case of `<Play>`, the noun is a URL of an audio file which 
Twilio will retrieve and play to the caller.

Twilio supports the following audio MIME types:

"Allowed audio Content-Types"
Mime-type | 	Description
:-------- | :---------
audio/mpeg	| mpeg layer 3 audio
audio/wav	| wav format audio
audio/wave	| wav format audio
audio/x-wav	| wav format audio
audio/aiff	| audio interchange file format
audio/x-aifc| 	audio interchange file format
audio/x-aiff| 	audio interchange file format
audio/x-gsm	| GSM audio format
audio/gsm	| GSM audio format
audio/ulaw	| &#956;-law audio format

### Nesting Rules {#nesting-rules}

The `<Play>` verb can be nested in the following elements:

* [`<Response>`](response)
* [`<Gather>`](gather)

The following verbs can be nested within `<Play>`:

* none

### Examples {#examples}

#### Example 1: Simple Play {#examples-1}

~~~
<?xml version="1.0" encoding="UTF-8" ?>
<Response>
	<Play>http://foo.com/cowbell.mp3</Play>
</Response>  
~~~

This TwiML document tells Twilio to download the cowbell.mp3 file and play the audio to the caller.

### Hints and Advanced Uses {#hints}

*	Twilio will attempt to cache the audio file the first time it is played.  
	This means the first attempt may be slow to play due to the time 
	spent downloading the file from your remote server.  Twilio may play 
	a processing sound while the file is being downloaded.
*	Twilio obeys standard HTTP caching headers.  If you change 
	a file already cached by Twilio, make sure your web server is sending the 
	proper headers to inform us that the contents of the file have changed.
*	Audio played over the telephone network is transcoded to a format
	the telephone network understands. Regardless of the quality of the file 
	you provide us, we will transcode so it plays correctly.  This may
	result in lower quality because the telephone number does not support
	high bitrate audio.  
*	High bitrate, lossy encoded files, such as 128kbps mp3 files, will take 
	longer to transcode and potentially sound worse than files that are in 
	lossless 8kbps formats.  This is due to the inevitable degradation that 
	occurs when converting from lossy compressed formats and the processing 
	involved in converting from higher bit rates to low bit rates.
