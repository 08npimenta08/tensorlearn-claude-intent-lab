# PRD N-17 — Results-ready notification
Status: APPROVED for discovery and specification, 2026-09-18. Owner: product owner.

## Problem and outcome
The fictional clinic receives avoidable calls from patients checking whether results are ready. Notify the patient when a result is ready for their review in the existing authenticated portal. Measure reduction in results-status calls and delivery failures. The baseline and numerical target will be agreed during discovery; none is approved yet.

## Approved constraints
- A doctor must review and release the result before any notification is eligible.
- The notification says that an update is available and directs the patient to the existing authenticated portal. No diagnosis, test result, clinical measurement or treatment detail goes in the message or notification logs.
- Authenticate access in the existing portal. Do not attach results or introduce a public result link.
- Avoid duplicate notifications for the same released result version; handle delivery failures without duplicate patient messages.

## Scope
Use the existing portal result route. Portal redesign, appointment scheduling, clinical interpretation and notifying third parties are outside this approved scope.

## Unresolved decisions
- Channel (SMS, email, portal push): product owner and privacy owner must decide after consent/delivery review.
- Notification to the referring doctor: clinical owner and privacy owner must decide recipient authorization. It is not approved by this PRD.
