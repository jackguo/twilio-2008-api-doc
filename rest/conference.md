<div id="version-info" class="alert">
    This document describes Twilio's old 2008-08-01 API. Please use the 
    <a href="/docs/api/rest">latest version</a>.
</div>

# REST API: Conferences

The Conferences list resource allows you to list the conferences within an account.
The Conference instance resource allows you to query and manage the state of individual
conferences. When a caller joins a conference via the TwiML `<Dial>` verb and `<Conference>`
noun, Twilio creates a Conference instance resource to represent the conference
room and a Participant instance resource to represent the caller
who joined.



### Conference Instance Resource

This resource represents an individual conference instance. Each conference
that starts and ends is considered a unique conference instance. There can be
many conference instances that share the same FriendlyName, but only one can
be in-progress at any given time.

#### Resource Properties

A `<Conference>` resource is represented by the following properties:

"Conference Resource Properties"
Property  | Description
:-------------	|:-------------
Sid |	A 34 character string that uniquely identifies this resource.
FriendlyName | A user provided string that identifies this conference room
Status | An integer representing the state of the conference 0=init, 1=in-progress, 2=completed
DateCreated  |	The date that this resource was created, given in RFC 2822 format.
DateUpdated  |	The date that this resource was last updated, given in RFC 2822 format.
AccountSid 	 | The 34 character id of the [Account][account] this Call is associated with. (Your account!)

[account]: account

#### HTTP Methods

##### GET

~~~
<TwilioResponse>
  <Conference>
    <Sid>CFd0a50bbe038c437e87f6c82db8f37f21</Sid>
    <AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
    <FriendlyName>1234</FriendlyName>
    <Status>2</Status>
    <DateCreated>Thu, 03 Sep 2009 23:37:53 -0700</DateCreated>
    <DateUpdated>Fri, 04 Sep 2009 00:35:02 -0700</DateUpdated>
  </Conference>
</TwilioResponse>
~~~

##### POST

Not supported

##### PUT

Not supported

##### DELETE

Not supported




### Conferences List Resource 

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/Conferences

#### HTTP Methods

##### GET

Returns a list of conferences made by your account. Each is represented by a `<Conference>` element, under a `<Conferences>` list element that includes [paging information][page]. The list is sorted by DateUpdated, with newest conferences first. Example: 

[page]: response#paging

~~~
<TwilioResponse>
  <Conferences page="0" numpages="1" pagesize="50" total="1" start="0" end="0">
    <Conference>
      <Sid>CFd0a50bbe038c437e87f6c82db8f37f21</Sid>
      <AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
      <FriendlyName>1234</FriendlyName>
      <Status>2</Status>
      <DateCreated>Thu, 03 Sep 2009 23:37:53 -0700</DateCreated>
      <DateUpdated>Fri, 04 Sep 2009 00:35:02 -0700</DateUpdated>
    </Conference>
  </Conferences>
</TwilioResponse> 
~~~

##### List Filtering 

You may limit the list by providing certain query string parameters to the listing resource. Note, parameters are case-sensitive:

* **Status:** Integer between 0 and 2. Only show calls currently in this status.
* **FriendlyName:** List conferences who's FriendlyName is the exact match of this string
* **DateCreated:** Only show conferences that started on this date, given as YYYY-MM-DD. Example: DateCreated=2009-07-06. You can also specify inequality, such as DateCreated<=YYYY-MM-DD for conferences that started at or before midnight on a date, and DateCreated>=YYYY-MM-DD for conferences that started at or after midnight on a date.
* **DateUpdated:** Only show conferences that were last updated on this date, given as YYYY-MM-DD. Example: DateUpdated=2009-07-06. You can also specify inequality, such as DateUpdated<=YYYY-MM-DD for conferences that were last updated at or before midnight on a date, and DateUpdated>=YYYY-MM-DD for conferences that were updated at or after midnight on a date.

Examples:

> /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Conferences?Status=1&FriendlyName=MyRoom

Would show the in progress conference named "MyRoom"

> /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Conferences?Status=2&DateCreated=2009-07-06

Would only show completed conferences started on Jul 06th, 2009.

> /2008-08-01/Accounts/AC309475e5fede1b49e100272a8640f438/Conferences?Status=1&DateCreated>=2009-07-06

Would only show in progress conferences started on or after midnight Jul 06th, 2009.

##### POST

Not supported

##### PUT

Not supported

##### DELETE

Not supported









## REST API: Participants

The Participants list resource allows you to query and manipulate the participants connected to a conference.





### Participant Instance Resource

This resource represents a single conference participant, identified by a CallSid.

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/Conferences/{ConferenceSid}/Participants/{CallSid}

#### Resource Properties

A Participant resource is represented by the following properties:

"Participant Resource Properties"
Property  | Description
:-------------	|:-------------
CallSid |	A 34 character string that uniquely identifies the call that is connected to this conference
ConferenceSid | A 34 character string that identifies the conference this participant is in
DateCreated  |	The date that this resource was created, given in RFC 2822 format.
DateUpdated  |	The date that this resource was last updated, given in RFC 2822 format.
AccountSid 	 | The 34 character id of the [Account][account] this Call is associated with. (Your account!)
Muted | Is this participant currently muted. Either "true" or "false".
StartConferenceOnEnter | Was the startConferenceOnEnter attribute set on this participant. Either 1 (true) or 0 (false).
EndConferenceOnExit | Was the endConferenceOnExit attribute set on this participant. Either 1 (true) or 0 (false).

[account]: account

#### HTTP Methods

##### GET 

Returns a representation of the Conference status of this participant:

~~~
<TwilioResponse>
  <Participant>
    <ConferenceSid>CF9f2ead1ae43cdabeab102fa30d938378</ConferenceSid>
    <AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
    <CallSid>CA9ae8e040497c0598481c2031a154919e</CallSid>
    <Muted>false</Muted>
    <StartConferenceOnEnter>1</StartConferenceOnEnter>
    <EndConferenceOnExit>0</EndConferenceOnExit>
    <DateCreated>Wed, 31 Dec 1969 16:33:29 -0800</DateCreated>
    <DateUpdated>Wed, 31 Dec 1969 16:33:29 -0800</DateUpdated>
  </Participant>
</TwilioResponse>
~~~

##### POST {#muting}

Update the status of a participant. Updatable statuses are:


"POST Updatable Participant Resource Properties"
Property  | Description
:-------------	|:-------------
Muted | Is this participant currently muted. Either true or false. Anything besides true or false (or 1 or 0) will be interpreted as false.

For example, you can POST to mute or unmute a participant:

> POST /2008-08-01/Accounts/AC5ea872f.../Conference/CF9f2ead1ae43cdabe.../Participants/CA9ae8e04049... HTTP/1.1
> Muted=true

Returns the participant with its new, `<Muted>true</Muted>` status:
~~~
<TwilioResponse>
  <Participant>
    <ConferenceSid>CF9f2ead1ae43cdabeab102fa30d938378</ConferenceSid>
    <AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
    <CallSid>CA9ae8e040497c0598481c2031a154919e</CallSid>
    <Muted>true</Muted>
    <StartConferenceOnEnter>1</StartConferenceOnEnter>
    <EndConferenceOnExit>0</EndConferenceOnExit>
    <DateCreated>Wed, 31 Dec 1969 16:33:29 -0800</DateCreated>
    <DateUpdated>Wed, 31 Dec 1969 16:33:29 -0800</DateUpdated>
  </Participant>
</TwilioResponse>
~~~

> POST /2008-08-01/Accounts/AC5ea872f.../Conference/CF9f2ead1ae43cdabe.../Participants/CA9ae8e04049... HTTP/1.1
> Muted=false

Returns the participant with its new, `<Muted>false</Muted>` status:

~~~
<TwilioResponse>
  <Participant>
    <ConferenceSid>CF9f2ead1ae43cdabeab102fa30d938378</ConferenceSid>
    <AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
    <CallSid>CA9ae8e040497c0598481c2031a154919e</CallSid>
    <Muted>false</Muted>
    <StartConferenceOnEnter>1</StartConferenceOnEnter>
    <EndConferenceOnExit>0</EndConferenceOnExit>
    <DateCreated>Wed, 31 Dec 1969 16:33:29 -0800</DateCreated>
    <DateUpdated>Wed, 31 Dec 1969 16:33:29 -0800</DateUpdated>
  </Participant>
</TwilioResponse>
~~~

##### PUT

Not supported

##### DELETE

Kick this participant from the conference. Returns HTTP 204 (No Content), with no body, if the participant was successful booted from the conference.

Example:

> DELETE /2008-08-01/Accounts/AC309475e5fe.../Conference/CF0123456789ABCDEF0123456.../Participants/CA0123456789ABC... HTTP/1.1



### Participants List Resource 

#### Resource URI

> /2008-08-01/Accounts/{YourAccountSid}/Conferences/{ConferenceSid}/Participants

#### HTTP Methods

##### GET 

Returns a list of Participants in a conference

~~~
<TwilioResponse>
  <Participants page="0" numpages="1" pagesize="50" total="2" start="0" end="1">
    <Participant>
      <ConferenceSid>CF9f2ead1ae43cdabeab102fa30d938378</ConferenceSid>
      <AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
      <CallSid>CA9ae8e040497c0598481c2031a154919e</CallSid>
      <Muted>false</Muted>
      <StartConferenceOnEnter>1</StartConferenceOnEnter>
      <EndConferenceOnExit>0</EndConferenceOnExit>
      <DateCreated>Wed, 31 Dec 1969 16:33:29 -0800</DateCreated>
      <DateUpdated>Wed, 31 Dec 1969 16:33:29 -0800</DateUpdated>
    </Participant>
    <Participant>
      <ConferenceSid>CF9f2ead1ae43cdabeab102fa30d938378</ConferenceSid>
      <AccountSid>AC5ea872f6da5a21de157d80997a64bd33</AccountSid>
      <CallSid>CA3c3c002e06c4cdaa5367e814ade05c76</CallSid>
      <Muted>false</Muted>
      <StartConferenceOnEnter>1</StartConferenceOnEnter>
      <EndConferenceOnExit>0</EndConferenceOnExit>
      <DateCreated>Wed, 31 Dec 1969 16:33:29 -0800</DateCreated>
      <DateUpdated>Wed, 31 Dec 1969 16:33:29 -0800</DateUpdated>
    </Participant>
  </Participants>
</TwilioResponse>
~~~

##### List Filtering 

You may limit the list by providing certain query string parameters to the
listing resource. Note, parameters are case-sensitive:

* **Muted:** Only show participants that are muted or unmuted.  Example: Muted=true or Muted=false 

##### POST

Not supported

##### PUT

Not supported

##### DELETE

Not supported
