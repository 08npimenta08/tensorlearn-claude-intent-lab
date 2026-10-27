# Write the intent

A fictional clinic wants fewer calls asking whether results are ready. The approved PRD, recent notes and a superseded request disagree on scope and recipients.

Here, an **intent** is a short document written before any spec or code: the problem, the outcome you want, who and what it affects, what is out of scope, the constraints, and the questions still open and who owns them. A **superseded** request has been replaced by a later approved decision: it is history, not current approval. The three sources are in `inputs/`: the approved PRD, the latest discovery notes and the superseded early request.

**What you'll practise:** capturing a request as `intent.md` with Claude Code. Let Claude interview you, have it write the draft, then check every line against the sources and correct what it got wrong. The traps are in what Claude may take from the superseded request.

**In the course slides:** Chapter 1, Lesson 1 (Capture as intent.md): the same results-ready scenario, the same six sections, and the same flow of questions first, then the draft, then your corrections.

## Task

1. Read every file in `inputs/` and identify which decisions are approved, superseded or still open.
2. Ask Claude to interview you about the request before it writes anything: scope, users, constraints and success. Answer from the approved sources.
3. Ask Claude to write `intent.md` with these sections: Problem, Proposed outcome (including how success will be observed), Affected users and systems, Out of scope, Constraints, and Open questions with their owners.
4. Read the draft against `inputs/`, correct what Claude got wrong or took from the superseded request, then submit.

A good way to work, as in the course: start with *"Before writing anything, ask me questions about this request until you understand it. Then write intent.md."* Answer from `inputs/` only, and ask Claude to quote the file behind each decision. When you check the draft, look for the usual mistakes: describing the solution instead of the problem, empty constraints, and open questions that were quietly decided. Numbering your items (for example I-1 and OPEN-1) makes them easy to trace in the next lab, *Make the spec and plan traceable*.

## Working rules

Only `intent.md` may change. Claude may write the draft; you are responsible for every line in it. Clinical-detail disclosure or bypassing doctor review is a critical error that caps the result at 25/100.

## Acceptance targets

- The approved outcome and review-before-notification rule are explicit.
- Messages expose no clinical details; unauthorized recipients are not silently approved.
- Portal redesign and other superseded scope are excluded.
- Channel choice and referring-doctor notifications remain open with owners.
- The intent gives observable success conditions without inventing a target or approval.

## Done when

- The `<!-- Write your intent here. -->` placeholder is gone, and `intent.md` contains no HTML comment and no TODO.
- `intent.md` has the six sections, states how success will be observed, and keeps the open questions with their owners.
- `intent.md` ends with exactly one ```json decision record that uses only the listed values.
- Nothing in it is approved only by the superseded request, and no numeric target is invented.

## Graded format

- The prose (everything outside the JSON block) must be at least 400 characters and must state the scope, what is out of scope (or excluded), the success conditions and the owners.
- The referring doctor is decided separately, as an open question with its own owners. It is not part of `third-party-notifications`.

## Decision record (graded)

End `intent.md` with exactly one JSON block that records your decisions. Choose values
only from these lists:

- `notifyWhen` (every condition that must be true before a notification): `lab-upload-complete`, `doctor-reviewed`, `released`, `result-abnormal`
- `messageMayInclude` (everything the approved message may contain): `update-available`, `test-name`, `result-value`, `diagnosis`, `treatment`
- `inScope`: `existing-portal-route`, `duplicate-prevention`, `delivery-failure-handling`, `portal-redesign`, `appointment-scheduling`, `clinical-interpretation`, `third-party-notifications`, `patient-dashboard`
- `openDecisions[].topic`: `channel`, `referring-doctor-notification`, `portal-redesign`
- `openDecisions[].owners`: `product`, `privacy`, `clinical`, `engineering`, `support`
- `successMeasures`: `status-only-calls`, `delivery-failures`, `all-inbound-calls`, `clinical-questions`
- `numericTarget`: a number, or `null` if none is approved

```json
{
  "notifyWhen": [],
  "messageMayInclude": [],
  "inScope": [],
  "openDecisions": [{"topic": "", "owners": []}],
  "successMeasures": [],
  "numericTarget": null
}
```

The prose is still the main deliverable: the record only captures what you decided.
Replace the `<!-- ... -->` placeholder in `intent.md` (leftover comments or `TODO` fail
the prose check) and use exactly one fenced `json` block. A missing or invalid record
caps the score at 25, like notifying too early or allowing clinical detail.

## How to submit

1. Fork this repository to your personal GitHub account. Use fictional information
   only, keep the fork public, and keep `main` as its default branch. In your fork's
   **Actions** tab, click **I understand my workflows, go ahead and enable them** if shown.
2. On the lab page, use **Connect GitHub for scoring** to install the read-only grading
   App on your fork, then **Recheck connection**.
3. Download the TensorLearn connector from the lab page and extract it outside your fork.
   From your fork folder, run the page's `--code` connection command and paste your token
   only into the hidden terminal prompt. This opens a Claude Code session connected to the lab.
4. In that session, run `/load`. Run `/load` again after your instructor launches the round.
5. Create a branch such as `intent/first-draft` and complete the task above. Change only the
   files the task allows: any other change, including `CLAUDE.md`, `.claude/` settings or
   agents, or notes files, makes the submission invalid.
6. Commit and push your branch. Open a PR **inside your own fork**, targeting its `main`
   branch (GitHub may suggest the upstream repository: select your fork instead). Check
   that **Check intent submission** starts in the PR's checks. Keep the PR open; no merge
   is required.
7. Run **`/submit`** in the connected Claude Code session, then `/check` (or refresh the
   lab page) to read your saved score. The Actions run is useful feedback, but it does not
   submit your work.

A change to your answer needs a new commit, push and `/submit`. Keep `main` unchanged:
the grader compares your PR with the released starter.

## What is graded

The pinned grader in `.github/workflows/grade.yml` reads your files as data and runs your
code only in an isolated, offline container. Every check is a fixed rule; no AI model
judges your work. Your score is out of 100. Your facilitator separately reviews
clarity and reasoning.

- intent.md has substantive prose covering purpose, scope, exclusions, success and open decisions.
- The decision record states the notification gate, allowed message content and approved scope.
- Both open decisions keep their owners, no other decision is invented, and no numeric target is invented.
- The success measures are the observable ones the sources name.
- A missing or invalid decision record, notifying before review and release, or allowing clinical detail in the message caps the score at 25/100.
