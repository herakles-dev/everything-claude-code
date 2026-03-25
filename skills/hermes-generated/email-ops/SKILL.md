---
name: email-ops
description: Evidence-first mailbox triage and sent-mail-safe reply workflow for Hermes. Use when organizing folders, drafting or sending through Himalaya, or verifying a message landed in Sent.
origin: Hermes
---

# Email Ops

Use this when the user wants Hermes to clean a mailbox, move messages between folders, draft or send replies, or prove a message landed in Sent.

## Prerequisites

Before using this workflow:
- install and configure the Himalaya CLI for the target mailbox accounts
- confirm the account's Sent folder name if it differs from `Sent`

## Skill Stack

Pull these companion skills into the workflow when relevant:
- `investor-outreach` when the email is investor, partner, or sponsor facing
- `search-first` before assuming a mail API, folder name, or CLI flag works
- `eval-harness` mindset for Sent-folder verification and exact status reporting

## When To Use

- user asks to triage inbox or trash, rescue important mail, or delete only obvious spam
- user asks to draft or send email and wants the message to appear in the mailbox's Sent folder
- user wants proof of which account, folder, or message id was used

## Workflow

1. Read the exact mailbox constraint first. If the user says `himalaya only` or forbids Apple Mail or `osascript`, stay inside Himalaya.
2. Resolve account and folder explicitly:
   - check `himalaya account list`
   - use `himalaya envelope list -a <account> -f <folder> ...`
   - never misuse `-s INBOX` as a folder selector
3. For triage, classify before acting:
   - preserve investor, partner, scheduling, and user-sent threads
   - move only after the folder and account are confirmed
   - permanently delete only obvious spam or messages the user explicitly authorized
4. For replies or new mail:
   - read the full thread first
   - choose the sender account that matches the project or recipient
   - compose non-interactively with piped `himalaya template send` or `message write`
   - avoid editor-driven flows unless required
5. If the request mentions attachments or images:
   - resolve the exact absolute file path before broad mailbox searching
   - keep the task on the local send-and-verify path instead of branching into unrelated web or repo exploration
   - if Mail.app fallback is needed, pass the attachment paths after the body: `osascript /Users/affoon/.hermes/scripts/send_mail.applescript "<sender>" "<recipient>" "<subject>" "<body>" "/absolute/file1" ...`
6. If the user wants an actual send and Himalaya fails with an IMAP append or save-copy error, fall back to `/Users/affoon/.hermes/scripts/send_mail.applescript` only when the user did not forbid Apple Mail or `osascript`, then verify Sent. If the user constrained the method to Himalaya only, report the exact blocked state instead of silently switching tools.
7. During long-running mailbox work, send a short progress update before more searching. If a budget warning says 3 or fewer tool calls remain, stop broad exploration and spend the remaining calls on the highest-confidence execution or verification step, or report exact status and next action.
8. If the user wants sent-mail evidence:
   - verify via `himalaya envelope list -a <account> -f Sent ...` or the account's actual sent folder
   - report the subject, recipient, account, and message id or date if available
9. Report exact status words: drafted, sent, moved, flagged, deleted, blocked, awaiting verification.

## Pitfalls

- do not claim a message was sent without Sent-folder verification
- do not use the wrong account just because it is default
- do not delete uncertain business mail during cleanup
- do not switch tools after the user constrained the method
- do not wander into unrelated searches while an attachment path or Sent verification is unresolved
- do not keep searching through the budget warning while the user is asking for a status update

## Verification

- the requested messages are present in the expected folder after the move
- sent mail appears in Sent for the correct account
- the final report includes counts or concrete message identifiers, not vague completion language
