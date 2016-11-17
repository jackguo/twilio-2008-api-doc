<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/twiml">latest version</a>.
</div>

# TwiML <span class="docs-tm">TM</span> Voice: Response Element

The root element of Twilio's XML Markup is the `<Response>` element. 
All of Twilio's verb elements must be nested within this element. Any other structure is 
considered invalid.

### Response Element Attributes {#attributes}

Attribute Name	|Allowed Values	|Default Value
:-------------	|:-------------	|:------------
[version](#version) 	|	Twilio version identifier	|	your accounts default version

#### version {#version}

The version attribute tells the Twilio engine which version of the software to 
use to process the TwiML elements within the document. This is useful to developers who would 
like their application to run under a legacy version of Twilio, rather than 
the latest release. The current Twilio releases are:

* 2008-08-01

### Nesting Rules {#nesting-rule}

The `<Response>` element is the top level element and may not be nested in any other verb element.

The following verbs can be nested within `<Response>`:

*  [`<Say>`](say)
*  [`<Play>`](play)
*  [`<Gather>`](gather)
*  [`<Record>`](record)
*  [`<Dial>`](dial)
*  [`<Sms>`](sms)
*  [`<Redirect>`](redirect)
*  [`<Pause>`](pause)
*  [`<Hangup>`](hangup)


### Examples {#examples}

#### Example 1: Simple Response {#examples-1}

~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response>
     <Say>Hello</Say>
</Response> 
~~~

An simple response example with a nested [`<Say>`](say) verb.

#### Example 2: Response with version number  {#examples-2}
~~~
<?xml version="1.0" encoding="UTF-8"?>
<Response version="2008-08-01">
     <Say>Hello</Say>
</Response>
~~~

A response element specifying its preferred version.
