# Invariants

Statements true regardless of what this kit adds. Changes propose; invariants veto.

## One source
- The method is written once, in `SKILL.md`. `README.md` summarises it, `adapters/` point at it, `docs/method.md` explains it. No second copy of a rule.
- `SKILL.md` names no tool first, and carries no frontmatter or directive that works in only one tool.

## Portability
- Every file in the kit is plain markdown or POSIX sh. Nothing here requires a package manager, a network call, or a particular assistant.
- `bin/gravity-init` refuses when `./gravity/` exists and never overwrites a file.

## Size
- Each template stays under a page. Each adapter stays under ten lines. Each of this kit's own gravity files stays under sixty lines.

## Ownership
- A project's copies of the templates are the project's. A change to a template here is a suggestion, never an update pushed to anyone.
- Nothing in the kit refers to the private project it was extracted from, or to any one person's tooling.

## Versions
- Tags are semver. Every tag has a `CHANGELOG.md` entry. A template change is at least a minor version.

---
*Adding one: it belongs here only if violating it would be a bug in every part of the kit, not just one file.*
