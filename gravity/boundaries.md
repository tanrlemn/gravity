# Boundaries

What exists and what each part owns.

## Modules

### SKILL.md
**Owns:** the method — bootstrap, spec, write-back, standing rules.
**Exposes:** the text a tool loads as a skill, or a person pastes as instructions.
**May not:** describe installation per tool, or carry anything a project would copy.

### templates/
**Owns:** the shape of the five gravity files and the spec.
**Exposes:** six files to copy.
**May not:** contain filled values, or refer to this repo.

### adapters/
**Owns:** pointing a tool at a project's gravity layer.
**Exposes:** one file per tool.
**May not:** restate the method, or exceed ten lines.

### README.md
**Owns:** the problem in a paragraph, the five-minute start, installation per tool, the layout.
**May not:** be the only place a rule is written.

### docs/
**Owns:** explanation and the worked example in prose.
**May not:** contradict `SKILL.md`; where it would, `SKILL.md` changes first.

### gravity/
**Owns:** this kit's own standing state, and the demonstration of a filled layer after use — it is the example. *(An `examples/` module was planned and withdrawn 2026-10-09.)*
**May not:** be mistaken for a template; it is filled, and it is ours.

## Crossing rules
- The method flows one way: `SKILL.md` → `README.md` summary → `adapters/` pointer → `docs/` explanation. Never the reverse.
- A downstream fork of this skill syncs from here; nothing from a fork's private conventions flows back in.

---
*If a change requires crossing a line here, that is a boundary change: make it explicitly and update this file.*
