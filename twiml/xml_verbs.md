<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# XML Verbs

### Twilio's Simple XML Instruction Set {#xml-verbs}

This is the real exciting part. When your application responds to Twilio 
with XML, we treat it like a set of actions to perform. Verbs! Here's 
what you need to know:

* **Root Element**: The root document element is always `<Response>`
* **XML Elements**: Are case-sensitive (as XML is), and words are capitalized, such as `<Response>` (not `<response>`)
* **XML Attributes**: Are case-sensitive (as XML is), and are "camelCased," beginning with a lower case letter. For example, maxLength instead of maxlength, MaxLength, or MAXLENGTH
* **XML Comments**: Just like HTML comments, feel free to use them liberally in your XML to help you debug. `<!-- COMMENTS HERE -->`
* **Verbs**: Include one or more of the verbs listed below.
* **Nesting**: Some of the verbs can be nested inside each other. Read below to find out more...

Here are the core five Twilio Verbs...

* **[`<Say>`](say)**: Say some text to the caller
* **[`<Play>`](play)**: Play an audio file to the caller
* **[`<Gather>`](gather)**: Gather phone keypad digits and submit them back to my application
* **[`<Dial>`](dial)**: Dial another phone number and connect the current call if someone picks up
* **[`<Record>`](record)**: Record the caller's voice and submit the recording back to my application

