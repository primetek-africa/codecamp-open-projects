# SafeTrip Check-In Architecture

## Shape
A mobile web client talks to a secured API and database, with a notification adapter for reminders. Location sharing is user-triggered and not continuously collected.

## Main modules and records
- User account and trusted contact consent.
- Trip plan: destination area, expected time, status.
- Check-in events and reminder schedule.
- Notification attempts and user-controlled retention.

## Main flow
The user starts a trip and chooses contacts. The app sends the trip plan, reminds the user to check in, and contacts the selected people only if the user misses the configured check-in.

## Operations and safeguards
Keep trip data private, expire it promptly, and make sharing explicit and revocable. Rate-limit notifications and state clearly that the app is not an emergency service.
