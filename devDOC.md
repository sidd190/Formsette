
### Problem Statement
To create a reusable forms alternative for hosting events/recruitments that's kinda understands campus workflows

### Human Actors
-> Participants/registrants
-> Organizers
    - Host(event owner)
    - team members added(reviewers/check in volunteers)
-> instance admin hosting the server

A person can have multiple roles

### System Actors
-> Identity provider
-> SMTP
-> Message gateway
-> Object storage
-> Gforms/club hours portal

### Core Use case
This is basically - forms are rotated (people fill these out) -> cleaned up and shortlisting done -> emails/messages sent -> rsvp happens -> get a report of total people coming -> event day (check in through unique QRs with a guy scanning) -> automatically marks attendance -> cleanup the list and save at the end -> convert to required club hours format for the portal ---- edit any time in between

### Alternative flows
- shortlisting/no shortlisting
- walk ins/no walkins
- no-show
- connectivity faliure
- notification faliure

### Functional Requirements

#### Registrant

- discover/open event
- register
- view registration
- recieve confirmation
- rsvp
- check in credentials
- see attendance state if needed

#### Event Organizer

- create event
- create registration form
- publish registration
- view responses
- shortlist
- change selection state
- send communication
- configure attendance policy
- view attendance
- export required records

#### Reviewer

- see permitted applications
- score/review
- shortlist or recommend

#### Check in volunteer

- access event check-in
- scan attendee
- verify registration
- record attendance
- handle walk-in

#### Organization admin

- manage organization members
- manage roles
- view previous events
- configure defaults


### Non Goals
Not a college ERP. We are not replacing attendance ERP, academic records, fees, timetable systems, etc.
Not a generic Google Forms clone. Forms are the entry point; our focus is event/application workflows.
Not replacing official college processes. If club-hours require a faculty-signed sheet and XLSX upload, we support that workflow rather than pretending it doesn't exist.
Not a full CRM.
Not an LMS.
Not a payment platform.
Not a WhatsApp/email replacement. We integrate with communication channels.
Not biometric attendance / face recognition.
Not a spreadsheet/database builder like Airtable.
Not initially designed for millions of concurrent users.
Not microservices-first. Architecture should remain as simple as our requirements allow.

### Quality Attributes

#### Performance

pehla tu -
During a normal event registration period, a user submitting a valid form should receive acknowledgement within <1 second at p95, excluding uploaded-file transfer time.

dooja tu -
During event entry, up to 500 attendees may attempt check-in within 10 minutes. Check-ins should normally complete within 500 ms p95 without duplicate attendance records

#### availability

pehla tu -
During registration and event check-in periods, the service should remain usable despite failures in non-critical external systems such as SMTP.

#### Durability

pehla tu -
Once the system returns "registration successful," that registration must survive application crashes and ordinary server restarts.

#### Security

pehla tu - cross tenancy
A member authorized for Organization A must never be able to access submissions belonging exclusively to Organization B.

dooja tu - attendance fraud
Knowledge or possession of another attendee's check-in information should not by itself allow unauthorized attendance to be recorded

teeja tu - Privilege
A check-in volunteer may mark attendance but cannot export all event responses, modify the event, or manage organization members.

#### idempotency. accessibility. maintainability. recoverability

Repeated QR scans, API retries or browser retries must not create duplicate attendance records or registrations

A failed deployment or server crash should be recoverable without losing committed registration/attendance data.

A user must be able to complete core registration workflows using keyboard navigation and assistive technology.

A contributor should be able to run the complete development environment locally without knowledge of the production infrastructure.


