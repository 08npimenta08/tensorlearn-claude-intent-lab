# Intent — Results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18) and `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). The superseded early request is history, not approval, and nothing below relies on it.

## Problem

- **P-1** The clinic receives avoidable calls from patients who only want to know whether their results are ready. PRD: "The fictional clinic receives avoidable calls from patients checking whether results are ready."
- **P-2** Today patients have no signal that a reviewed result is waiting for them in the portal, so they phone to ask. The phone calls are the symptom; the missing "your result is ready" signal is the problem.

## Proposed outcome

- **O-1** When a doctor has reviewed and released a result, the patient is told that an update is available and is pointed to the existing authenticated portal to view it. PRD: "Notify the patient when a result is ready for their review in the existing authenticated portal."
- **O-2** "Ready" means doctor-reviewed and released, not that the laboratory upload finished. Product owner: "keep the PRD's review-before-notification gate. 'Ready' must not mean that the laboratory upload merely finished."

**How success will be observed**

- **S-1** Fewer status-only calls ("are my results ready?"), counted separately from clinical questions and compared against a measured baseline. PRD: "Measure reduction in results-status calls and delivery failures." Support lead: "count status-only calls separately from clinical questions; compare against a measured baseline."
- **S-2** Notification delivery failures are tracked and handled without sending duplicate messages to the patient. PRD: "Measure reduction in results-status calls and delivery failures."
- **S-3** No numeric target is set. The baseline and target are agreed during discovery. PRD: "The baseline and numerical target will be agreed during discovery; none is approved yet."

## Affected users and systems

- **U-1** Patients, the only approved recipients. They receive the notification and view their result in the existing portal.
- **U-2** Doctors. Their review-and-release step is the gate that makes a notification eligible (PRD, "Approved constraints").
- **U-3** Support / call-centre staff. They handle the calls and categorise status-only calls versus clinical questions for measurement (support lead, discovery notes).
- **U-4** Product, privacy, clinical and engineering owners. They own the open decisions and constraints below.
- **SYS-1** The existing authenticated patient portal and its existing result route. Engineering lead: "the existing portal path is adequate." PRD: "Use the existing portal result route."
- **SYS-2** The result review/release workflow, the source of the "released" event.
- **SYS-3** The notification delivery mechanism and its operational logs. The channel is still open (OPEN-1).

## Scope

In scope (PRD, "Scope" and "Approved constraints"):

- **I-1** Notify the patient through the existing portal result route once a result has been doctor-reviewed and released.
- **I-2** Duplicate prevention: "Avoid duplicate notifications for the same released result version."
- **I-3** Delivery-failure handling: "handle delivery failures without duplicate patient messages."

## Out of scope

- **X-1** Portal redesign. PRD: "Portal redesign ... [is] outside this approved scope."
- **X-2** A new patient dashboard. Engineering lead: "A new patient dashboard is a separate project."
- **X-3** Appointment scheduling. PRD: outside approved scope.
- **X-4** Clinical interpretation of results. PRD: outside approved scope.
- **X-5** Notifying third parties. PRD: "notifying third parties [is] outside this approved scope." The referring doctor is not approved as a recipient; it is kept as a separate open question (OPEN-2).
- **X-6** Attaching results to the message, or any public result link. PRD: "Do not attach results or introduce a public result link."
- **X-7** Treating lab upload completion as "ready". Product owner, discovery notes.

## Constraints

- **C-1 Review before notification.** "A doctor must review and release the result before any notification is eligible." (PRD) A notification is eligible only when the result is both doctor-reviewed and released.
- **C-2 No clinical detail.** The message only says that an update is available and directs the patient to the portal. "No diagnosis, test result, clinical measurement or treatment detail goes in the message or notification logs." (PRD) Privacy owner: "no sensitive content in messages or operational logs."
- **C-3 Authenticated access only.** "Authenticate access in the existing portal. Do not attach results or introduce a public result link." (PRD)
- **C-4 Idempotent delivery.** At most one notification per released result version, and retries after delivery failures must not create duplicate patient messages (PRD).
- **C-5 Recipients.** Only the patient is approved. No other recipient may be added without the owners' decision in OPEN-2. Clinical owner: "do not infer approval for referring-doctor notifications from the old request."
- **C-6 No invented target.** Success is reported against a measured baseline; no numeric target exists until one is agreed (PRD).

## Open questions and owners

- **OPEN-1 Channel (SMS, email, portal push).** Owners: product owner and privacy owner, "after consent/delivery review" (PRD). Privacy owner: "Consent and the actual channel are still under review." Not decided here.
- **OPEN-2 Notification to the referring doctor.** Owners: clinical owner and privacy owner, who "must decide recipient authorization. It is not approved by this PRD." (PRD) Clinical owner: "do not infer approval for referring-doctor notifications from the old request." Not decided here.
- **OPEN-3 Baseline and numeric target.** To be agreed during discovery (PRD). Not decided here; no target is assumed.

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
