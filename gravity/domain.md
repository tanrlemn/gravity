# Domain

One canonical name per concept. The synonym list is load-bearing.

## Gravity layer
**Is:** the five standing files in a project's `/gravity/` — job, domain, invariants, boundaries, grammar. Edited, never rewritten.
**Is not:** a spec, an instructions file, a prompt.
**Never call it:** context files, the rules, the docs.

## Spec
**Is:** one dated file describing one change, grounded in the gravity layer, disposable once shipped.
**Is not:** a gravity file; a ticket; a plan for the whole project.
**Never call it:** PRD, story, feature doc.

## Write-back
**Is:** the step after a change lands that records in the gravity layer what the work decided.
**Is not:** a changelog entry or a commit message.
**Never call it:** update, sync, cleanup.

## Template
**Is:** one of the six files in `templates/` that a project copies to start its own gravity layer or spec. Owned by the project once copied.
**Is not:** a file a project reads from this repo at run time.
**Never call it:** schema, boilerplate, starter.

## Adapter
**Is:** a pointer file of at most ten lines in `adapters/`, one per tool, that sends the assistant to the project's gravity layer.
**Is not:** a copy of the method or of the gravity files.
**Never call it:** config, integration, plugin.

## Skill
**Is:** `SKILL.md` — the method in Agent Skills form, the single source of truth in this repo.
**Is not:** tool-specific; it carries no frontmatter that works in one tool and vanishes in the rest.
**Never call it:** the prompt, the system prompt.

## Example
**Is:** a filled gravity layer in `examples/` that has been lived in — a `TBD` closed by a dated ruling, a spec with its write-back filled.
**Is not:** prose about the method, or a template with sample values.

---
## Relationships
A project copies Templates once and owns the copies. The Skill describes how a Gravity layer is bootstrapped, how a Spec is grounded against it, and how a Write-back keeps it true. An Adapter points a tool at the Gravity layer and nothing else. An Example shows one Gravity layer after use.
