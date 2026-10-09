# Interaction Grammar

How the UI behaves, decided once, at the system level. Fill this in even if design is not the
priority — these decisions get made either way, and the alternative is making them differently
on every screen.

## Tokens
**Type:** <display / body / mono, with sizes>
**Color:** <background, surface, text, accent, danger>
**Spacing scale:**
**Radius / borders / shadow:**

## States — every data view has all four
**Loading:** <skeleton / spinner / nothing under Nms>
**Empty:** <illustration? copy tone? does it offer the primary action?>
**Error:** <inline or toast; is retry offered; what is shown to the user vs logged>
**Partial:** <what happens when some data loads and some fails>

## Actions
**Primary action position:**
**Destructive actions:** <confirm dialog / type-to-confirm / undo window>
**Async feedback:** <optimistic or pending; where the indicator lives>

## Forms
**Validation timing:** <on blur / on submit>
**Error display:** <under field / summary at top>
**Required-field marking:**

## Explicit fallback rule
When a case is not covered here, do not substitute a default — ask, then add the answer to
this file.
