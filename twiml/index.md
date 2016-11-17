<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# TwiML <span class="docs-tm">TM</span>: the Twilio Markup XML

### Overview {#overview}

Twilio handles call and sms based instructions in realtime from web applications.

When an sms or incoming call is received, Twilio looks up the URL of the web application associated 
with the phone number called and makes a request to that URL. The web server that
responds to the request decides how the call should proceed by returning a Twilio Markup XML (TwiML) 
document telling Twilio to say text to the caller, send an sms message, play audio files, 
get input from the keypad, record audio, connect the call to another phone and more.  TwiML is 
similar to a HTML in that just as HTML is rendered in a browser to display a webpage, 
TwiML is 'rendered' by the Twilio to the caller.  Only one TwiML document is rendered to the caller 
at once but many documents can be linked together to build complex interactive voice applications.  

Outgoing calls and SMS messages are controlled in the same manner as incoming calls using TwiML.  
The initial URL for the call is provided as a parameter to the Twilio [REST API](/docs/api/rest) 
method used initiate the call.  See the [making calls](/docs/api/rest/making_calls)
section of the REST documentation for more information on initiating outgoing calls. 
See the [sending SMS messages](/docs/api/rest/sending-sms) section of the REST documentation for more
information on sending SMS messages.

### How Twilio Interacts with Your Application {#twilio-interaction}

The first TwiML document Twilio fetches for a call is requested using an HTTP POST.  Subsequent call
flow changes such as gathering input from the keypad using 
[`<Gather>`][gather] or recording audio using
[`<Record>`][record] provides a 'method' parameter that
allows an application to select either HTTP GET or POST.

[gather]: /docs/api/2008-08-01/twiml/gather
[record]: /docs/api/2008-08-01/twiml/record

#### POST {#twilio-interaction-post}
When Twilio performs an HTTP POST to a web server, metadata associated with the call (caller, etc.)
is passed as the body of the POST just as if the parameters were submitted as part of the form on
a webpage.  One important caveat with POSTs is that Twilio cannot cache POSTs.  If you want Twilio
to cache static TwiML pages then have Twilio make requests using GETs. 

#### GET {#twilio-interaction-get}
When Twilio performs an HTTP GET to a web server, metadata associated with the call (caller, etc.)
is passed as URL query string parameters as if the parameters were submitted as part of
a form on a webpage.  Twilio will honor standard HTTP caching directives for HTTP GET requests
and locally cache static TwiML documents and media files.  Caching is especially important for
large media files like MP3s. 



