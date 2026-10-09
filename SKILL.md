---
name: gravity
description: Maintains a project's standing "gravity layer" (job, domain vocabulary, invariants, boundaries, interaction grammar) and grounds every feature spec against it. Use this skill whenever starting a new project or codebase, writing or reviewing a feature spec, adding a feature, planning a refactor, or any time the user mentions requirements, specs, scope, architecture, or that a build feels sloppy, drifting, inconsistent, or bloated. Also use it before generating substantial application code, since ungrounded generation is the main source of drift.
license: MIT
metadata:
  author: Tanner Lemon
  version: 0.1.0
---

# Gravity

Feature specs go stale and codebases drift because each spec is locally coherent but globally unanchored. The fix is a small standing layer that every spec is checked against.

## The two layers

**Gravity layer** — standing state. Lives in `/gravity/`. Edited, never rewritten. Five files:

| File | Holds | Fails as |
|---|---|---|
| `job.md` | Why the system exists, who is in pain | Feature creep with no way to say no |
| `domain.md` | Every noun, one canonical name, one definition | Three schemas for the same concept |
| `invariants.md` | Statements true regardless of feature | Silent contradictions between features |
| `boundaries.md` | Modules and what each owns | Business logic in view components |
| `grammar.md` | How the UI behaves across all screens | Every screen re-decides loading and errors |

**Specs** — changes. They live in `/specs/`, one per change, dated, disposable once shipped.

Keeping these separate is the whole point. Treating everything as ephemeral prompt material is what produces slop.

## Pick a mode

Look for `/gravity/`. Missing → **Bootstrap**. Present → **Spec**.

## Bootstrap mode

Copy the templates from `templates/` into `/gravity/`, then fill them **in order** — each one depends on the last. Interview the user; do not invent answers. If they don't know yet, write `TBD` rather than a plausible guess, because a plausible guess becomes load-bearing.

1. **job.md** — Ask: who is in pain, what do they do today instead, what would make them stop using this? Two or three sentences. This is the tiebreaker later, so keep it specific enough to rule things out.
2. **domain.md** — Extract every noun from the conversation. For each, pick one canonical name and kill the synonyms explicitly. The synonym list matters as much as the definition: it is what stops a later session from inventing `item` when the project says `artwork`.
3. **invariants.md** — Ask what must always be true. Push for the boring ones: identity, ownership, money representation, tenancy, deletion semantics, time zones. These are cheap to write and expensive to discover late.
4. **boundaries.md** — Name the modules and what each owns. Then state what is *not* allowed to cross.
5. **grammar.md** — Loading, empty, error, destructive confirmation, primary action placement, form validation timing. Fill this even if the user says they don't care about design. Especially then — it is the decision set that otherwise gets made silently and differently every session.

Keep each file under roughly 60 lines. Long context files get skimmed, and a skimmed invariant is no invariant.

Then make sure whatever file your agent reads at session start points at the five files rather than duplicating them:

```markdown
# Project

Read these before writing or changing code:
- gravity/job.md
- gravity/domain.md
- gravity/invariants.md
- gravity/boundaries.md
- gravity/grammar.md

Specs live in /specs/, one per change, dated.
```

Tools that support file imports can inline them; tools that do not will follow the paths. Either way the content lives in one place.

## Spec mode

Read all five gravity files first, then write the spec using `templates/spec.md`.

The spec is not ready until both are true:

- It names at least one invariant or job statement it serves.
- It has been checked against every other invariant and violates none.

If neither can be satisfied, say so plainly. A feature that serves nothing in the gravity layer is either unnecessary or a sign the gravity layer is missing something — and those are different problems with different fixes. Ask which one it is instead of picking.

Use only canonical names from `domain.md`. If the spec needs a noun that doesn't exist yet, stop and add it to `domain.md` first. Introducing vocabulary in a spec is how drift starts.

## After a change lands

Write back to the gravity layer. This is the step that gets skipped, and skipping it is what makes the layer go stale until nobody trusts it.

- New concept named during the work → `domain.md`
- New rule that must hold from now on → `invariants.md`
- New module, or a boundary that moved → `boundaries.md`
- New UI pattern that other screens should follow → `grammar.md`
- Purpose meaningfully narrowed or widened → `job.md`

Then delete or archive the spec. It has served its purpose; leaving shipped specs around invites a future session to treat them as current truth.

## Standing rules while generating code

- Never introduce a synonym for an existing domain noun.
- Never place logic outside the boundary that owns it, even when the file currently open is a convenient place for it.
- When a decision is required that the gravity layer doesn't cover, stop and ask rather than choosing. Then record the answer in the right file. An unrecorded decision will be re-decided differently next session.
- When asked to build something that violates an invariant, surface the conflict instead of quietly working around it. The invariant may be wrong — but that is a change to make deliberately.

## Surfaces

The gravity files are plain markdown in the repo, so they work anywhere. Whatever file your agent reads at session start — `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, a Copilot or Cursor rules file, a project's knowledge files — points at the five files. The `adapters/` folder in this repo has one such pointer per tool; copy the one you need to the place your tool reads it.

## Templates

In `templates/`: `job.md`, `domain.md`, `invariants.md`, `boundaries.md`, `grammar.md`, `spec.md`. Copy, don't reference — each project owns its own copies. A later change to a template here is a suggestion to your project, not an update to it.
