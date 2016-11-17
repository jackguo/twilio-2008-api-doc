<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# TwiML <span class="docs-tm">TM</span> Voice: Pause Verb

The `<Pause>` verb waits silently for a number of seconds.

### Verb Attributes {#attributes}

The `<Pause>` verb supports the following attributes that modify its behavior:

Attribute Name	|Allowed Values	|Default Value
:-------------	|:-------------	|:------------
[length](#length) |	integer > 0  |	1 second

#### length {#length}

The length attribute specifies how many seconds Twilio will wait silently.

### Nouns {#nouns}

`<Pause>` does not have any expected "noun" body values.

### Nesting Rules {#nesting-rules}

The `<Pause>` verb can be nested in the following elements

* [`<Response>`](response)
* [`<Gather>`](gather)

The following verbs can be nested within `<Pause>`

* none

### Examples {#examples}

#### Example 1: Simple pause {#examples-1}

~~~
<?xml version="1.0" encoding="UTF-8" ?>
<Response>
	<Say>I will pause 10 seconds starting now!</Say>
	<Pause length="10"/>
	<Say>I just paused 10 seconds</Say>
</Response>
~~~

This example demonstrates using `<Pause>` to wait between two Say verbs.
