# gravity

A standing layer that keeps AI-built code from drifting.

Feature specs go stale and codebases drift because each spec is locally coherent but globally unanchored. An assistant writing feature 12 has no reliable way to know what features 1–11 decided, so it re-decides. A second name appears for a concept that already had one. Business logic lands in whatever file happens to be open. The fix is five short files that every change is checked against, and a habit of writing back to them when a change lands.

Model-agnostic: it works with any assistant that can read files in a repository. The method is `SKILL.md`, an [Agent Skill](https://agentskills.io). Everything else here points at it.

## The two layers

**Gravity** is standing state. It lives in `/gravity/`, is edited and never rewritten, and is five files:

| File | Holds | Fails as, when missing |
|---|---|---|
| `job.md` | Why the system exists, who is in pain | Feature creep with no way to say no |
| `domain.md` | Every noun, one canonical name, one definition | Three schemas for the same concept |
| `invariants.md` | Statements true regardless of feature | Silent contradictions between features |
| `boundaries.md` | Modules and what each owns | Business logic in view components |
| `grammar.md` | How the UI behaves across all screens | Every screen re-decides loading and errors |

**Specs** are changes. They live in `/specs/`, one per change, dated, and are disposable once shipped.

## Five-minute start

By hand, in your project's root:

1. Copy `templates/job.md`, `domain.md`, `invariants.md`, `boundaries.md` and `grammar.md` into `gravity/`. Copy `templates/spec.md` wherever you keep specs.
2. Copy the adapter for your tool from `adapters/` to the place your tool reads it (table below), or append its lines to the instructions file you already have.
3. Install the skill (next section), or paste the "How it runs" part of `SKILL.md` into your tool's custom instructions.
4. Open a session and say "bootstrap the gravity layer". The assistant interviews you and fills the five files in order. Where you don't know yet, it writes `TBD`.

Or run `bin/gravity-init` from your project root; it does steps 1 and 2 and refuses if `gravity/` already exists.

## Installing the skill

The Agent Skills spec fixes the file format but not where a tool looks for project skills. Copy `SKILL.md` and `templates/` into the folder your tool reads. As of this writing:

| Tool | Project skills folder | Instructions file (adapter) |
|---|---|---|
| Claude Code | `.claude/skills/gravity/` | `CLAUDE.md` → `adapters/CLAUDE.md` |
| Codex | `.codex/skills/gravity/` | `AGENTS.md` → `adapters/AGENTS.md` |
| Gemini CLI | `.gemini/skills/gravity/` | `GEMINI.md` → `adapters/GEMINI.md` |
| Cursor | `.cursor/skills/gravity/` | `.cursor/rules/gravity.mdc` → `adapters/cursor-rule.mdc` |
| GitHub Copilot | see your tool's docs | `.github/copilot-instructions.md` → `adapters/copilot-instructions.md` |
| Anything else | see your tool's docs | any file it reads at session start → `adapters/AGENTS.md` |

Check your tool's documentation for the current path; adopters are listed at agentskills.io. If your tool has no skills folder, the method still works: the instructions file points at the five gravity files, and `SKILL.md` is short enough to paste.

## What is here

```
SKILL.md          the method — the single source of truth
templates/        job, domain, invariants, boundaries, grammar, spec
adapters/         one pointer file per tool, each under ten lines
bin/gravity-init  optional POSIX sh: copy templates, write or append the pointer
docs/method.md    the method explained, with a worked example
gravity/          this kit's own gravity layer
CHANGELOG.md
```

## Versions

Tags follow semver. A change to a template here is a suggestion to your project, not an update to it: your copies are yours, and the write-backs in them are the record of your project, not of this one.

## License

MIT. Written by Tanner Lemon.
