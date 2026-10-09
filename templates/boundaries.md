# Boundaries

What exists and what each part owns. This is what keeps logic from landing wherever the
currently-open file happens to be.

## Modules

### <module>
**Owns:** <the decisions and data only this module makes>
**Exposes:** <the surface others may call>
**May depend on:** <modules>
**May not:** <the tempting violation>

### <module>
**Owns:**
**Exposes:**
**May depend on:**
**May not:**

## Crossing rules
- Business rules live in <layer>, never in <layer>.
- <External service> is reached only through <adapter>.
- Direction of dependency: <a> -> <b>, never the reverse.

---
*If a change requires crossing a line here, that is a boundary change: make it explicitly and
update this file, rather than making an exception.*
