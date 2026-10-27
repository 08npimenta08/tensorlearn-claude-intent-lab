# Write the intent

A fictional clinic wants fewer calls asking whether results are ready. The approved PRD, recent notes and a superseded request disagree on scope and recipients.

## Task

1. Read every file in `inputs/` and identify which decisions are approved, superseded or still open.
2. Ask the read-only coach clarification questions.
3. Personally write `intent.md` in your editor. Include purpose, scope, exclusions, success conditions and unresolved decisions.
4. Request a read-only review, revise your own draft, then submit.

## Working rules

Only `intent.md` may change. Claude can read and ask questions but has no write, shell, agent or mutation-MCP tools. Write the draft yourself. Coaching prompts prohibit replacement prose; the final file alone cannot prove human authorship. Clinical-detail disclosure or bypassing doctor review is a critical error: the announced rubric caps the result at 25/100.

## Acceptance targets

- The approved outcome and review-before-notification rule are explicit.
- Messages expose no clinical details; unauthorized recipients are not silently approved.
- Portal redesign and other superseded scope are excluded.
- Channel choice and referring-doctor notifications remain open with owners.
- The intent gives observable success conditions without inventing a target or approval.

## Decision record (graded)

End `intent.md` with exactly one JSON block that records your decisions. Choose values
only from these lists:

- `notifyWhen`: `lab-upload-complete`, `doctor-reviewed`, `released`, `result-abnormal`
- `messageMayInclude`: `update-available`, `test-name`, `result-value`, `diagnosis`, `treatment`
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

## How to submit

1. Fork this repository to your personal GitHub account. Use fictional information
   only, keep the fork public, and keep `main` as its default branch.
2. Clone your fork and enable GitHub Actions in its **Actions** tab. Use **Connect
   GitHub for scoring** on the lab page to install the read-only grading App on your
   fork. Return to the lab and use **Recheck connection** if needed.
3. Download the TensorLearn client from the lab page and extract it outside your fork.
   Follow the page's connection command from your fork folder. Paste your learner
   token only into the hidden terminal prompt. Run `/load` from your fork folder to
   connect the repository. Run `/load` again after the facilitator launches the round.
4. Create a branch such as `intent/first-draft` and complete the task above. Change
   only the files the task allows; any other change makes the submission invalid.
5. Commit and push your branch. Open a PR **inside your own fork**, targeting its
   `main` branch. GitHub may suggest the upstream repository: select your fork instead.
   Opening or updating this PR starts the checks automatically.
6. Keep the PR open. When ready, run **`/submit`** in the connected Claude Code session
   to submit that exact pushed revision for the current round. **No merge is required.**
7. Use `/check` or refresh the lab page to read the saved result. The Actions run and
   `lab-score/score.json` are useful feedback, but an Actions check alone does not
   submit your work or prove that the server saved a score.

A change to your answer requires a new commit, push, and explicit `/submit`. Keep
`main` unchanged: the grader compares your PR with the released starter.

## What is graded

The pinned grader in `.github/workflows/grade.yml` reads your files as data and runs
your code only in an isolated, offline container. Every check is a fixed rule; no AI
model judges your work. Your facilitator separately reviews clarity and reasoning.

- intent.md has substantive prose covering purpose, scope, exclusions, success and open decisions.
- The decision record states the notification gate, allowed message content and approved scope.
- Both open decisions keep their owners, no other decision is invented, and no numeric target is invented.
- The success measures are the observable ones the sources name.
- Notifying before review and release, or allowing clinical detail in the message, caps the score at 25/100.
