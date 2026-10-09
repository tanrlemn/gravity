# Grammar

This kit has no UI. Its grammar is how its documents behave, decided once.

## Voice
- Second person for the reader ("your project"), third person for the assistant ("the assistant"), never a vendor's model name in the method.
- Short declaratives. A rule is one sentence; its reason, when given, is the next.

## Structure
- Every markdown file opens with one `#` heading naming the file's job, then `##` sections. No deeper than `###`.
- Tables for the five files and for installation paths; prose everywhere else.
- Paths in backticks. Commands in fenced blocks. Nothing linked that the reader cannot reach from a fresh clone, except agentskills.io.

## Dates and versions
- Dates are `YYYY-MM-DD`. A dated line says what happened on that date, not what is true now.
- A ruling written into a gravity file is appended in italics with its date and never edits the line it rules on.

## Empty, missing, unknown
- A value not yet decided is written `TBD`, never guessed and never left blank.
- An install path we have not verified is marked "see your tool's docs" rather than invented.

## Destructive actions
- `bin/gravity-init` never overwrites and never deletes. Removing a project's gravity layer is a thing people do by hand.

## Explicit fallback rule
When a case is not covered here, do not substitute a default — ask, then add the answer to this file.
