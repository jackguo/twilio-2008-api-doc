<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# TwiML <span class="docs-tm">TM</span> Voice: Say Verb

The `<Say>` verb converts text to speech that is read back to the caller. `<Say>` is useful for development or saying dynamic text that is difficult to pre-record.

### Verb Attributes {#attributes}

The `<Say>` verb supports the following attributes that modify its behavior:

Attribute Name	|Allowed Values	|Default Value
:-------------	|:-------------	|:------------
[voice](#voice)	| man, woman  |	man
[language](#language) |	en, es, fr, de | 	en
[loop](#loop) |	integer >= 0 |	1

#### voice {#voice}

The voice attribute allows you to choose a male or female voice to read the text back.  The default value is "man". 	

#### language {#language}

The language attribute allows you pick a voice with a specific language's accent and pronunciations.  The currently 
supported languages are "en" (English), "es" (Spanish), "fr" (French), and "de" (German). The default is "en".

#### loop {#loop}

The loop attribute specifies how many times you'd like the text repeated. The default is once.  
Specifying 0 will cause the the &lt;Say&gt; verb to loop until the call is hung up.

### Nouns {#nouns}

The "noun" of a Twilio verb is the text body of the XML element: the thing the verb
acts upon. In the case of `<Say>`, the noun is the text you wish spoken to the
caller. There is a 4KB limit on the text that `<Say>` can process.

### Nesting Rules {#nesting-rules}

The `<Say>` verb can be nested in the following elements:

* [`<Response>`](response)
* [`<Gather>`](gather)

The following verbs can be nested within `<Say>`:

* none

### Examples {#examples}

#### Example 1: Hello World  {#examples-1}

~~~
<?xml version="1.0" encoding="UTF-8" ?>
<Response>
     <Say>Hello World</Say>
</Response>
~~~

When a call is directed to the following TwiML document, the caller will hear "hello world" spoken once in a male voice.

#### Example 2: Hello, Hello  {#examples-2}

~~~
<?xml version="1.0" encoding="UTF-8" ?>
<Response>
     <Say voice="woman" loop="2">Hello</Say>
</Response>
~~~

This TwiML document tells Twilio to say Hello twice in a row with a female voice to the caller.

### Hints and Advanced Uses  {#hints}

*	When translating text to speech, the &lt;Say&gt; tag will 
	make assumptions about how to pronounce numbers, 
	dates, times, amounts of money, and other abbreviations. 
	
*	When saying numbers: 12345 will be spoken as "twelve thousand three hundred forty five". 1 
	2 3 4 5 will be spoken as "one two three four five". 
	
*	Punctuation, such as commas and periods will be interpreted as natural pauses by the speech engine. 
	
*	&lt;Say&gt; is useful for saying dynamic text that would be difficult to pre-record.  In cases where
	the contents of &lt;Say&gt; are static, you might consider recording a live person saying the phrase 
	and using the [`<Play>`](play) verb instead. 
	
*	If you want to insert a longer pause try using the [`<Pause>`](pause)
	verb.  &lt;Pause&gt; should be placed outside &lt;Say&gt; tags not nested inside them.

