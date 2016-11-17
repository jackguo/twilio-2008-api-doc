<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# TwiML <span class="docs-tm">TM</span> Voice: Hangup Verb

The `<Hangup>` verb ends a call.

### Verb Attributes {#attributes}

The `<Hangup>` verb has no attributes.

### Nouns {#nouns}

`<Hangup>` does not have any expected "noun" body values.
 
### Nesting Rules {#nesting-rules}

The `<Hangup>` verb can be nested in the following elements:

* [`<Response>`](response)

The following verbs can be nested within `<Hangup>`:

* none

### Examples {#example}
 
#### Example 1: Simple hang up {#examples-1}

~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
     <Hangup/>
</Response>   
~~~

The following code causes the call to hang up immediately

### Hints and Advanced Uses {#hints}
 
* 	After processing a Gather, Record, or Dial, you may end the call by sending
	back a response containing the `<Hangup>` verb.
