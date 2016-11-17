<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# TwiML <span class="docs-tm">TM</span> SMS: Redirect Verb

The `<Redirect>` verb transfers control to a different URL.

### Verb Attributes {#attributes}

The `<Redirect>` verb supports the following attributes that modify its behavior:

Attribute Name	|Allowed Values	|Default Value
:-------------	|:-------------	|:------------
[method](#method) 	|	GET, POST	|	POST

#### method {#method}

The method attribute takes the value GET or POST. This tells Twilio 
whether to request the `<Redirect>` URL via HTTP GET or POST. POST is the default.

### Nouns {#nouns}

The "noun" of a Twilio verb is the body of the element, the thing the verb
acts upon. In the case of `<Redirect>`, the noun is an absolute or
relative URL for a different TwiML document.

### Nesting Rules {#nesting-rules}

The `<Redirect>` verb can be nested within the following elements:

* [`<Response>`](response)

The following verbs can be nested within `<Redirect>`:

* none

### Examples {#examples}

#### Example 1: Absolute URL Redirect  {#examples-1}

~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
     <Redirect>
     	http://www.foo.com/nextInstructions
     </Redirect>
</Response>
~~~

In this example, we have a `<Redirect>` verb. `<Redirect>` makes a request to http://www.foo.com/nextInstructions and
transfers control to a different URL. 


#### Example 2: Relative URL Redirect  {#examples-2}
~~~ 
<?xml version="1.0" encoding="UTF-8"?> 
<Response> 
    <Redirect> 
        ../nextInstructions 
    </Redirect> 
</Response> 
~~~ 
Redirects flow control to a the specified URL relative to the current URL.
