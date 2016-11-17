<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: Short Codes

A short code is a 5 or 6-digit number that can send and receive SMS messages with mobile phones. These high-throughput numbers are perfect for apps that need to send SMS messages to lots of users or need to send time sensitive messages. You can buy shortcodes from Twilio or port existing short codes to our platform.

To send SMS messages from your short code, see the [Sending SMS][sendsms] documentation.

### ShortCode Instance Resource {#instance}

#### Resource URI {#instance-uri}

> /2008-08-01/Accounts/{AccountSid}/SMS/ShortCodes/{ShortCodeSid}

#### Resource Properties {#instance-properties}

Property     | Description
:------------ |:----------------
Sid          | A 34 character string that uniquely identifies this resource.
DateCreated  | The date that this resource was created, given as GMT [RFC 2822][13] format.
DateUpdated  | The date that this resource was last updated, given as GMT [RFC 2822][13] format.
FriendlyName | A human readable descriptive text for this resource, up to 64 characters long. By default, the `FriendlyName` is just the short code.
AccountSid   | The unique id of the [Account][account] that owns this short code.
ShortCode  | The short code. e.g., 894546.
ApiVersion		| SMSs to this short code will start a new TwiML session with this API version.
SmsUrl       | The URL Twilio will request when receiving an incoming SMS message to this short code.
SmsMethod    | The HTTP method Twilio will use when making requests to the `SmsUrl`. Either `GET` or `POST`.
SmsFallbackUrl| The URL that Twilio will request if an error occurs retrieving or executing the TwiML from `SmsUrl`.
SmsFallbackMethod| The HTTP method Twilio will use when requesting the above URL. Either `GET` or `POST`.

[account]: account

#### HTTP GET {#instance-get}

##### Example {#instance-get-example-1}

> GET /2008-08-01/Accounts/ACdc5f1.../SMS/ShortCodes/SC6b20cb705c1e8f00210049b20b70fce2

~~~
<TwilioResponse>
  <ShortCode>
    <Sid>SC6b20cb705c1e8f00210049b20b70fce2</Sid>
    <AccountSid>AC1c238e6f7a0441659833ca940b72503d</AccountSid>
    <FriendlyName>67898</FriendlyName>
    <ShortCode>67898</ShortCode>
    <DateCreated>Sat, 09 Jul 2011 22:36:22 +0000</DateCreated>
    <DateUpdated>Sat, 09 Jul 2011 22:36:22 +0000</DateUpdated>
    <SmsUrl>http://smsapp.com/sms</SmsUrl>
    <SmsMethod>POST</SmsMethod>
    <SmsFallbackUrl>http://smsapp.com/fallback</SmsFallbackUrl>
    <SmsFallbackMethod>GET</SmsFallbackMethod>
  </ShortCode>
</TwilioResponse>
~~~

#### HTTP POST {#instance-post}

Tries to update the short code's properties, and returns the updated resource representation if successful. The returned response is identical to that returned above when making a GET request.

##### Optional Parameters {#instance-post-optional-parameters}

You may specify one or more of the following parameters to update this short code's respective properties:

Parameter     | Description
:------------ | ----------------------------------------------------- |
FriendlyName | A human readable description of the short code, with maximum length 64 characters.
ApiVersion	| SMSs to this short code will start a new TwiML session with this API version. Either `2010-04-01` or `2008-08-01`.
SmsUrl | The URL that Twilio should request when somebody sends an SMS to the short code.
SmsMethod | The HTTP method that should be used to request the `SmsUrl`. Either `GET` or `POST`.
SmsFallbackUrl | A URL that Twilio will request if an error occurs requesting or executing the TwiML at the `SmsUrl`.
SmsFallbackMethod | The HTTP method that should be used to request the `SmsFallbackUrl`. Either `GET` or `POST`.

##### Example 1 {#instance-post-example-1}

Set the SmsUrl on a short code to 'http://myapp.com/awesome'

> POST /2008-08-01/Accounts/ACdc5f1.../SMS/ShortCodes/SC6b20cb705c1e8f00210049b20b70fce
> SmsUrl=http://myapp.com/awesome

~~~
<TwilioResponse>
  <ShortCode>
    <Sid>SC6b20cb705c1e8f00210049b20b70fce2</Sid>
    <AccountSid>AC1c238e6f7a0441659833ca940b72503d</AccountSid>
    <FriendlyName>67898</FriendlyName>
    <ShortCode>67898</ShortCode>
    <DateCreated>Sat, 09 Jul 2011 22:36:22 +0000</DateCreated>
    <DateUpdated>Wed, 13 Jul 2011 02:07:05 +0000</DateUpdated>
    <SmsUrl>http://myapp.com/awesome</SmsUrl>
    <SmsMethod>POST</SmsMethod>
    <SmsFallbackUrl>http://smsapp.com/fallback</SmsFallbackUrl>
    <SmsFallbackMethod>GET</SmsFallbackMethod>
  </ShortCode>
</TwilioResponse>
~~~

#### HTTP PUT {#instance-put}

Not supported.


#### HTTP DELETE {#instance-delete}

Not supported.


### ShortCodes List Resource {#list}

#### Resource URI {#list-uri}

> /2008-08-01/Accounts/{AccountSid}/SMS/ShortCodes

#### HTTP GET {#list-get}

Returns a list of short code resource representations, each representing a short code within your account. The list includes [paging information][14].

##### List Filters {#list-get-filters}

The following query string parameters allow you to limit the list returned. Note, parameters are case-sensitive:

Parameter | Description
--------- | -----------
ShortCode| Only show the ShortCode resources that match this pattern. You can specify partial numbers and use '*' as a wildcard for any digit.
FriendlyName| Only show the ShortCode resources with friendly names that exactly match this name.

##### Example 1 {#list-get-example-1}

> GET /2008-08-01/Accounts/ACdc5f1.../SMS/ShortCodes

~~~
<TwilioResponse>
  <ShortCodes page="0" numpages="1" pagesize="50" total="10" start="0" end="9">
    <ShortCode>
      <Sid>SC6b20cb705c1e8f00210049b20b70fce2</Sid>
      <AccountSid>AC1c238e6f7a0441659833ca940b72503d</AccountSid>
      <FriendlyName>67898</FriendlyName>
      <ShortCode>67898</ShortCode>
      <DateCreated>Sat, 09 Jul 2011 22:36:22 +0000</DateCreated>
      <DateUpdated>Wed, 13 Jul 2011 02:07:05 +0000</DateUpdated>
      <SmsUrl>http://myapp.com/awesome</SmsUrl>
      <SmsMethod>POST</SmsMethod>
      <SmsFallbackUrl>http://smsapp.com/fallback</SmsFallbackUrl>
      <SmsFallbackMethod>GET</SmsFallbackMethod>
    </ShortCode>
	...
  </ShortCodes>
</TwilioResponse>
~~~

##### Example 2 {#list-get-example-2}

Return the set of short codes that match exactly 67898.

> GET /2008-08-01/Accounts/ACdc5f1.../SMS/ShortCodes?ShortCode=67898

##### Example 3 {#list-get-example-3}

Return the set of all phone numbers containing the digits 867.

> GET /2008-08-01/Accounts/ACdc5f1.../SMS/ShortCodes?ShortCode=898

#### HTTP POST {#list-post}

Not supported.

#### HTTP PUT {#list-put}

Not supported.

#### HTTP DELETE {#list-delete}

Not supported.

 [1]: index
 [2]: request
 [3]: response
 [4]: account
 [5]: outgoing-caller-ids
 [6]: incoming-phone-numbers
 [7]: call
 [8]: making_calls
 [9]: recording
 [10]: transcription
 [11]: notification
 [12]: tips
 [13]: http://www.ietf.org/rfc/rfc2822.txt
 [14]: response#response-formats-list-paging-information
 [15]: /docs/errors/21452
 [16]: available-phone-numbers
 [e164]: http://en.wikipedia.org/wiki/E.164
 [sendsms]: sending-sms
