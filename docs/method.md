# Gravity: a standing layer that keeps AI-built code from drifting

*A short report on a working method. Model-agnostic: it works with any assistant that can read files in a repository.*

## The problem

Feature specs go stale and codebases drift because each spec is locally coherent but globally unanchored. An assistant writing feature 12 has no reliable way to know what features 1–11 decided — so it re-decides. A second name appears for a concept that already had one. Business logic lands in whatever file happens to be open. Every screen handles loading and errors its own way. None of these is a bug in a single feature; each is the absence of something above the features.

Treating everything as ephemeral prompt material is what produces this. The fix is a small standing layer that every change is checked against.

## The two layers

**Gravity** is standing state. It lives in `/gravity/` at the repo root, is edited and never rewritten, and is five files:

| File | Holds | Fails as, when missing |
|---|---|---|
| `job.md` | Why the system exists, who is in pain | Feature creep with no way to say no |
| `domain.md` | Every noun, one canonical name, one definition | Three schemas for the same concept |
| `invariants.md` | Statements true regardless of feature | Silent contradictions between features |
| `boundaries.md` | Modules and what each owns | Business logic in view components |
| `grammar.md` | How the UI behaves across all screens | Every screen re-decides loading and errors |

**Specs** are changes. They live in `/specs/`, one per change, dated, and are disposable once shipped.

Keeping these separate is the whole point. Gravity is small and slow; specs are many and fast. A spec that cannot name what in gravity it serves is either unnecessary or a sign gravity is missing something — and those are different problems with different fixes.

Each gravity file should stay under roughly 60 lines. Long context files get skimmed, and a skimmed invariant is no invariant.

## How it runs

**Bootstrap** — when `/gravity/` does not exist. Copy the templates into `/gravity/`, then fill them *in order*, because each depends on the last. The assistant interviews the owner; it does not invent answers. Where the owner does not know yet, the file says `TBD` rather than a plausible guess, because a plausible guess becomes load-bearing.

1. `job.md` — who is in pain, what do they do today instead, what would make them stop using this. Two or three sentences, specific enough to rule things out. This is the tiebreaker later.
2. `domain.md` — every noun from the conversation. One canonical name each; the synonyms killed explicitly. The synonym list matters as much as the definition.
3. `invariants.md` — what must always be true. Push for the boring ones: identity, ownership, money representation, tenancy, deletion semantics, time zones. Cheap to write, expensive to discover late.
4. `boundaries.md` — the modules and what each owns, then what is not allowed to cross.
5. `grammar.md` — loading, empty, error, destructive confirmation, primary-action placement, validation timing. Fill this even if design is not a priority. Especially then: it is the decision set that otherwise gets made silently and differently every session.

**Spec** — when `/gravity/` exists. Read all five files first, then write the spec from the template. The spec is not ready until both are true: it names at least one invariant or job statement it serves, and it has been checked against every other invariant and violates none. It uses only canonical names from `domain.md`; if it needs a noun that does not exist, stop and add the noun to `domain.md` first. Introducing vocabulary in a spec is how drift starts.

**Write-back** — after a change lands. This is the step that gets skipped, and skipping it is what makes the layer go stale until nobody trusts it.

- New concept named during the work → `domain.md`
- New rule that must hold from now on → `invariants.md`
- New module, or a boundary that moved → `boundaries.md`
- New UI pattern other screens should follow → `grammar.md`
- Purpose meaningfully narrowed or widened → `job.md`

Then archive the spec. Leaving shipped specs around invites a future session to read them as current truth.

**Standing rules while generating code**

- Never introduce a synonym for an existing domain noun.
- Never place logic outside the boundary that owns it, even when the open file is convenient.
- When a decision is required that gravity does not cover, stop and ask rather than choosing — then record the answer in the right file. An unrecorded decision will be re-decided differently next session.
- When asked to build something that violates an invariant, surface the conflict instead of quietly working around it. The invariant may be wrong, but that is a change to make deliberately.

## Wiring it to an assistant

The gravity files are plain markdown, so they work with anything. Whatever instructions file the assistant reads at the start of a session — a root `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, a project system prompt, or a project's knowledge files — should *import or point at* the five files rather than duplicate them:

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

Tools that support file imports can inline them; tools that do not will follow the paths. Either way the content lives in one place. If the assistant supports reusable skills or custom instructions, the "How it runs" section above is the skill, verbatim.

## A worked example

A small automated trading experiment, bootstrapped by interview on 2026-09-22 and in use since. The owner's words are quoted as spoken; everything else was written by the assistant from the interview and later rulings. Abridged.

### `job.md` as written at bootstrap

> **Who is in pain:** The owner. He wants an income-producing system that does not need him, and every existing path (manual trading, black-box bots, copy trading) either needs his time or hides whether it is actually working.
>
> **What he does today instead:** nothing in this space. The idea has been a thought, not a system.
>
> **What "solved" looks like:** a Paper Environment that has proven a net-of-fees edge over enough trades to trust; a Money Environment running only promoted strategies; a Log that grades the Coach's predictions against outcomes; a Skim arriving in the owner's hands; and the owner's involvement reduced to a periodic read of whether the record is measuring the right thing.
>
> **What would make him stop using this:** in his words, 2026-09-22 — "I would stop using it if it shows no sign of potential success in the next year." Deadline: 2027-09-22. What counts as a sign of potential success: **TBD** — must be defined in a spec before the Paper Environment goes live, so the stop condition is graded by the Log and not by feel.
>
> ---
> *Tiebreaker: this is a proof-of-edge machine before it is a money-maker. When a feature would make it earn faster but see itself less clearly, it does not ship.*

Three things to notice. The stop condition is the owner's sentence, not a paraphrase. The part he had not decided is marked `TBD`, with a deadline for deciding it, rather than filled with something reasonable. And the tiebreaker is specific enough to kill a feature that is individually sensible.

### Write-backs appended to the same file, later

> *Ruled 2026-09-22 (owner accepted the assistant's suggestion): a sign of potential success is either of — (a) Paper shows positive net-of-fees expectancy over at least 100 trades with drawdown under a ceiling set in the Paper v0 spec, or (b) the Coach's grade history shows its expectations match outcomes more often than not over at least 100 trades. Either is a sign; neither is proof.*
>
> *Ruled 2026-10-06: Paper starts at $100 USD, not $1. At $1 the venue's fee rounds up to a whole cent — ~100 bps a side against the 25 bps charged — and the Log measures cent resolution instead of edge. The $1 run (2026-09-22 → 2026-10-06, 27 closed trades, 0 winners, −54%) is kept whole as the record of that.*

The first closes the `TBD`. The second records a change to the founding premise *and the reason*, so the next session does not quietly reintroduce the $1 start because it read the original sentence.

### `invariants.md`, two sections

> **Log**
> - Append-only and hash-chained. Nothing in the system, Coach included, holds update or delete rights on it.
> - Before any clock acts, its expectation is written. After the world answers, the actual is written. Every Entry names its source.
> - Where the Ledger and the Log disagree, the Log is right and the Ledger is rebuilt from it.
>
> **Failure**
> - Venue unreachable: Money goes flat and opens nothing. Paper continues.
> - Log unreachable: nothing trades in either Environment.
> - Coach unreachable: the Incumbent runs unchanged. A missing Window is logged as "no change — Coach absent."

And a write-back on that file after the first spec shipped:

> *Write-back from the Paper v0 spec, applied 2026-09-23 (code landed 2026-09-22): the application role holds no UPDATE or DELETE on the Log; a trigger refuses both regardless of role, the owner included.*

### What skipping the step looks like

The session that shipped Paper v0 recorded, at its close, that the spec's write-back section "has not been applied to the gravity layer — code shipped, gravity files unchanged." It was applied the next day. Had it not been, `invariants.md` would have described a Log weaker than the one in production, and the next spec would have been checked against the wrong rules. The failure mode is quiet; the only defence is making write-back a named step that the session's close checks.

## What it costs and what it buys

Cost: an interview of five to eight questions at the start, a paragraph of write-back per shipped change, and reading five short files at the start of every session. Buys: one vocabulary across every session, a way to say no to a reasonable feature, and a record of why the system is shaped the way it is that does not live in anyone's memory — human or model.

---

## Appendix — the templates

The six templates are in `templates/` at the root of this repository.
