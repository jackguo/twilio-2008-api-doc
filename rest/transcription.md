<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: Transcriptions

A Transcription is a sub-resource of Recordings. It is the result of converting an audio recording to readable text. Transcriptions can be created via the TwiML Record Verb.

<div class="alert alert-error">
Note: transcription is a pay feature. If you request that an existing recording be transcribed, your account will be charged. See the pricing page for our transcription prices. Additionally, transcription is currently limited to 2 minute or less recordings. Requesting transcription on longer recording will return with a failure message.
</div>







### Transcription Instance Resource

This resource represents an individual transcription of a recorded call. 

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/Recordings/{RecordingSid}/Transcriptions/{TranscriptionSid}
 
By default Twilio will respond with the XML metadata for the Transcription. If you append ".txt" to the end of the Transcription resource's URI Twilio will just return you the transcription text:

> /2008-08-01/Accounts/{YourAccountSid}/Recordings/{RecordingSid}/Transcriptions/{TranscriptionSid}.txt

#### Resource Properties

A Transcription resource is represented by the following properties:

"Transcription Resource Properties"
 Property          | Description                                                                                                                           
 ----------------- | --------------
 Sid               | A 34 character string that uniquely identifies this resource.                                                                           
 DateCreated       | The date that this resource was created, given in [unix epoch][1] format.                                                                
 DateUpdated       | The date that this resource was last updated, given in [unix epoch][1] format.                                                           
 AccountSid        | The 34 character id of the [Account][2] this Transcription is associated with. (Your account!)                                         
 Status       | An string representing the status of the transcription. in-progress, completed, failed
 RecordingSid      | The 34 character id of the [Recording][3] this Transcription was made of.                                                              
 Duration          | duration of audio that was transcribed, in seconds                                                                                     
 TranscriptionText | The content of the transcription                                                                                                       
 Price             | The charge for this transcript in USD. Populated after the transcript is completed. Note, this value may not be immediately available.

#### HTTP Methods

##### GET 

Returns a single Transcription resource with the Sid provided. Example: 

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Recordings/RE42ed11f93dc08b952027ffbc406d0868/Transcriptions/TR42ed11f93dc08b952027ffbc406d0868 HTTP/1.1
~~~
<TwilioResponse>  
	<Transcription>
		<Sid>TRbdece5b75f2cd8f6ef38e0a10f5c4447</Sid>
		<DateCreated>1235986685</DateCreated>
		<DateUpdated>1235957924</DateUpdated>
		<AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
		<Status>completed</Status>
		<RecordingSid>RE3870404da563592ef6a72136438a879c</RecordingSid>
		<Duration>9</Duration>
		<TranscriptionText>
		    This is the body a transcribed recording
		</TranscriptionText>
		<Price>-0.03000</Price>
	</Transcription>
</TwilioResponse>
~~~

##### POST 

Not Supported 

##### PUT 

Not Supported 

##### DELETE 

Not Supported



### Transcriptions List Resource

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/Recordings/{RecordingSid}/Transcriptions

#### HTTP Methods

##### GET

> /2008-08-01/Accounts/{YourAccountSid}/Transcriptions  

Returns all the transcriptions made by your account. 

Each is represented by a `<Transcription>` element, under a `<Transcriptions>` list element that includes [paging information][4]. The list is sorted by DateUpdated, with newest transcripts first. Example: 

> GET /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Transcriptions HTTP/1.1
     
~~~
<TwilioResponse>

	<Transcriptions page="0" numpages="1" pagesize="50" total="2" start="0" end="1">

		<Transcription>
			<Sid>TR685e9a2bdf89b978491b1afada63f078</Sid>
			<DateCreated>1235986685</DateCreated>
			<DateUpdated>1235957975</DateUpdated>
			<AccountSid>AC309475e5fede1b49e100272a8640f438</AccountSid>
			<Status>completed</Status>
			<RecordingSid>RE3870404da563592ef6a72136438a879c</RecordingSid>
			<Duration>9</Duration>
			<TranscriptionText>
				This is the body of one transcribed recording
			</TranscriptionText>
			<Price>-0.25000</Price>
		</Transcription>
		
		<Transcription>
			<Sid>TRbdece5b75f2cd8f6ef38e0a10f5c4447</Sid>
			<DateCreated>1235986685</DateCreated>
			<DateUpdated>1235957924</DateUpdated>
			<AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
			<Status>completed</Status>
			<RecordingSid>RE3870404da563592ef6a72136438a879c</RecordingSid>
			<Duration>9</Duration>
			<TranscriptionText>
				This is the body of another transcribed recording
			</TranscriptionText>
			<Price>-0.03000</Price>
		</Transcription>
		...
	</Transcriptions>
</TwilioResponse>	
~~~
##### POST 

Not Supported 

##### PUT

Not Supported 

##### DELETE

Not Supported 




 [1]: http://en.wikipedia.org/wiki/Unix_time
 [2]: account
 [3]: recording
 [4]: response#paging
