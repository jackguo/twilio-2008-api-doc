<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: Sending SMS Messages

Since initiating outbound SMS messages via the REST API is such a frequent task, it get its own section. Huzza!

### POST to the SMS/Messages URI

To send a new outgoing SMS message, you perform an HTTP POST to your SMS/Messages list resource:

> /2008-08-01/Accounts/{YourAccountSid}/SMS/Messages

##### POST Parameters:

The following properties are used in your POST to send the SMS:

"Messages POST Parameters"
 Param      | Optional | Description
 :---------- | :-------- | :-------------
 From     	| Required | A number that you've acquired from Twilio to be used as sending number.  Note, only phone numbers purchased from Twilio work here, you cannot (for example) spoof SMS message from your own cell phone.
 To     	| Required | The phone number to call. Twilio will accept any format, such as (xxx) xxx-xxx, or xxx-xxx-xxxx. Pretty much anything. You may include a leading 1 or +1 for US phone numbers. Don't worry, we'll figure it out.  For calls outside the US, you must include the plus sign and country code.  For example: +44 for UK calls. This number can also be a [short code][shortcode].
 Body       | Required | The text you want to send, limited to 160 characters.
 StatusCallback | Optional | A URL that Twilio will POST to when your   message is processed.  An HTTP POST request will be made to this URL when the message is processed, posting the SmsSid as well as SmsStatus=sent or SmsStatus=failed.

### Examples

Sending an SMS from 415-555-1212 to 555-867-5309 asking Jenny if she wants to get dinner:

> POST /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/SMS/Messages HTTP/1.1
> From=415-555-1212&To=555-867-5309&Body=Hi+Jenny%21+want+to+grab+dinner%3F

~~~
<TwilioResponse>
    <SMSMessage>
        <Sid>SM872fb94e3b358913777cdb313f25b46f</Sid>
        <DateCreated>Sun, 04 Oct 2009 03:48:08 -0700</DateCreated>
        <DateUpdated>Sun, 04 Oct 2009 03:48:10 -0700</DateUpdated>
        <DateSent>Sun, 04 Oct 2009 03:48:10 -0700</DateSent>
        <AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
        <To>5558675309</To>
        <From>4155551212</From>
        <Body>Hi Jenny! Want to grab dinner?</Body>
        <Status>sent</Status>
        <Flags>2</Flags>
    </SMSMessage>
</TwilioResponse>
~~~

### Handling SMS Replies

By specifying an SMS URL for your SMS enabled Twilio phone number, Twilio will make a request to your application to notify you when someone replies to a message you send. [Twilio's request][request] and your corresponding [response][response] are covered in the [SMS portion][request] of the TwiML documentation.

[request]: /docs/api/2008-08-01/twiml/sms/twilio_request
[response]: /docs/api/2008-08-01/twiml/sms/your_response
[shortcode]: short-codes
