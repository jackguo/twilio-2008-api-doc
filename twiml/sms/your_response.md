<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# Responding to a Twilio SMS Request

### Twilio Is a Well Behaved HTTP Client  {#well-behaved-http-client}

Twilio behaves just like a web browser, so there's nothing new to learn.

* Cookies: Twilio accepts, and will return to you, HTTP cookies, just like a normal web browser
* Redirects: Twilio follows HTTP Redirects (HTTP status 301, 307, etc.), just like a normal web browser
* Caching: Twilio will cache HTTP files when headers allow it (via ETag and Last-Modified headers) and when the HTTP method is GET, just like a normal web browser

#### Cookies {#cookies}
Twilio will keep cookie state across multiple SMS messages between the same two phone
numbers. This allows you to treat the separate messages as a conversation, and store data
about the conversation, such as a session identifier, in the cookies for future reference. Twilio will expire the cookies for that conversation after four hours of inactivity.


### Twilio Understands Mime Types {#understands-mime-types}

Twilio does the right thing when your application responds with different mime types.

"MIME Types"
Mime Types | Behavior 
:-------------  |:------------- 
text/xml, application/xml, text/html | Twilio interprets the returned document as a TwiML XML Instruction Set. See the XML Verbs section for details. This is the most commonly used response. 
text/plain | Twilio returns the content of the text file to the sender in the form of a SMS message.

### Twilio's TwiML Interpreter  {#twiml-interpreter}

The root element of a TwiML document is always `<Response>`. When Twilio interprets a TwiML document it starts at the top and executes verbs in the order they are placed in the document. For example, Twilio will respond with a SMS message containing "Hello World".

~~~
<?xml version="1.0" encoding="UTF-8" ?>  
<Response> 
    <Sms>Hello World</Sms>
</Response>
~~~

You can provide multiple `<Sms>` verbs in one document to send multiple messages.  For example: 
~~~ 
<?xml version="1.0" encoding="UTF-8" ?>   
<Response>  
    <Sms>This is message 1 of 2.</Sms> 
    <Sms>This is message 2 of 2.</Sms> 
</Response> 
~~~

There are certain situations when elements in a TwiML document may not be reached because control flow has passed to a different document. For example, if a `<Redirect>` verb is followed by a `<Sms>`, the `<Sms>` will not be reached as Twilio will fetch TwiML at the specified redirect URL.

* [``<Sms>``](sms)
* [``<Redirect>``](redirect)



