<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# Responding to a Twilio Request

### Twilio Is a Well Behaved HTTP Client  {#well-behaved-http-client}

Twilio behaves just like a web browser, so there's nothing new to learn.

* Cookies: Twilio accepts, and will return to you, HTTP cookies, just like a normal web browser
* Redirects: Twilio follows HTTP Redirects (HTTP status 301, 307, etc.), just like a normal web browser
* Caching: Twilio will cache HTTP files when headers allow it (via ETag and Last-Modified headers) and when the HTTP method is GET, just like a normal web browser

### Twilio Understands Mime Types  {#understands-mime-types}

Twilio does the right thing when your application responds with different mime types.

* XML: Twilio interprets the returned document as an XML Instruction Set. See the XML Verbs section for details. This is the most commonly used response.
* Audio: Twilio plays the audio file and then hangs up. See the `<Play>` documentation for supported mime types.
* Raw Text: Twilio says the content of the text file, and then hangs up.

### Twilio's TwiML Interpreter {#twiml-interpreter}

When Twilio interprets a TwiML document it starts at the top and executes verbs in the order they are placed in the document. For example, in the following code snippet "Hello World" is read to the caller before the Cowbell.mp3 file is played for the caller. 

~~~
<?xml version="1.0" encoding="UTF-8" ?>  
<Response> 
    <Say>Hello World</Say>
    <Play>https://api.twilio.com/Cowbell.mp3</Play>
</Response>  
~~~

There are certain situations when elements in a TwiML document may not be reached because control flow has passed to a different document. For example, if a `<Say>` verb is followed by a `<Gather>` and then another `<Say>`, the 2nd `<Say>` may not be reached if the `<Gather>` action was called. The following verbs may impact control flow:

* [``<Gather>``](gather)
* [``<Record>``](record)
* [``<Dial>``](dial)
* [``<Redirect>``](redirect)
* [``<Hangup>``](hangup)



