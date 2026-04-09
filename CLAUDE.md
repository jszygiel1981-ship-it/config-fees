# Config Fees — CLAUDE.md
> Read by Claude Code at the start of every session in this project.

## Project

HTML prototype for the Configurable Fee Management feature in TRIO WFS.
PM: Cheyenne Metcalf | Status: Active Prototyping

**Project path:** `/Users/jesses/Projects/Trio/config-fees`
**Prototype A (Split Panel):** `prototype/index.html`
**Prototype B (Hierarchical Grid):** `prototype/alt.html`
**Prototype C (Decoupled Tabs):** `prototype/concept-c.html`
**Design system path:** `/Users/jesses/Projects/Trio/ahtg-design-system`
**Obsidian notes:** `My Vault/02-TRIO-WFS/Projects/Config Fees/`
**Figma file:** `kxcOpRSYW3vSHsHVdV9qPv` — use `outputMode: existingFile` for all captures

---

## What This Is

A configurable fee management interface inside TRIO WFS.
Users browse fee definitions and manage fee instances across AP and AR fee types.
The page is reused in three contexts: global, client-scoped, and agency-scoped.

---

## Three Layout Concepts (in exploration)

| Concept | File | Description |
|---------|------|-------------|
| **A — Split Panel** | `index.html` | Left panel = fee definitions (grouped, collapsible). Right panel = instances for selected definition. |
| **B — Hierarchical Grid** | `alt.html` | Single full-width grid. Definition rows expand to reveal a nested paper card with its own instance grid and column headers. |
| **C — Decoupled Tabs** | `concept-c.html` | Two independent grids: Fee Library tab (definitions) + Active Configurations tab (all instances). Linked by "N instances →" navigation and a definition filter dropdown. |

---

## Key Data Model

**Fee Definition fields:** Name, Description, Fee Type, Application Type, Group, Optional/Required, Active/Inactive
- On edit: Fee Type and Application Type are read-only (visible but locked)
- Cannot be deleted if any instances exist (past, current, or future)

**Fee Instance fields:** Fee Definition, Fee Value, Segment(s), Client, Agency, Start Date, End Date, Account Number
- Editable fields: End Date (if future or unset), Account Number
- Deletable: future instances only
- Category derived from scope: Global / Client-specific / Agency-specific / Client + Agency

**Date-state filters:** Current (default), Past, Future, All
- Current: startDate ≤ today AND (endDate ≥ today OR no end date)
- Past: endDate < today
- Future: startDate > today

**Overlap prevention:** No two instances with the same feeDefinitionId + segment + clientId + agencyId may have overlapping dates.
Before save, show: matching scope instances, conflict status, proposed auto-adjustments (plain language preview).

---

## Design Tokens

Always use tokens — never hardcode:

| Property | Value |
|----------|-------|
| Primary blue | `#2196F3` (`primary.main`) |
| Page background | `#F5F5F5` (`background.default`) |
| Surface / paper | `#FFFFFF` (`background.paper`) |
| Text primary | `#212121` |
| Text secondary | `#757575` |
| Divider / border | `#E0E0E0` |
| Spacing | `4 / 8 / 12 / 16 / 24 / 32 / 40px` only |
| Border radius | `4px` standard, `999px` pill |
| Font | Roboto |

The prototype links `../../ahtg-design-system/design-system-shell.css` for all tokens and shell styles.

---

## UX Principles (from spec)

- Dense, operational, not decorative. Avoid oversized controls and excessive whitespace.
- Default to compact definition browser (left panel). Expanded grid mode is available but not default.
- Scope/category must use consistent chip treatment — visually recognizable without relying on color alone.
- Empty states must be specific: distinguish "nothing exists" from "nothing matches this filter."
- View switching (By Fee ↔ All Instances) is a display-mode change, not a navigation change — feels lightweight.
- Editing is safe: immutable fields remain visible but disabled. No delete path on definitions.

---

## Behavior Rules

- Read existing prototype before modifying it
- Only touch files relevant to the current task
- No unsolicited refactors
- Figma file is `kxcOpRSYW3vSHsHVdV9qPv` — always use `outputMode: existingFile`, never create a new file
- Desktop-first — no responsive work unless explicitly requested
