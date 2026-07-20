# proj.md

This file is a project's memory - one page an AI assistant reads and updates so it
remembers this project between conversations. If you're setting this up for a new
project, copy this file, then follow the numbered steps in HOW TO USE below.

LINKS (core triad + ops): index = <slug>.index.md | readme/spec = proj.readme.md (SHARED - never fork) | ops = <slug>.ops.md | bootstrap how-to = proj.init.md

## STATE (authoritative volatile block - edit only these keys; JSON resists accidental over-editing; supersedes any version/updated in PROPS below)

```json
{
  "file": "<slug>.md",
  "version": 0,
  "updated": "DD-MM-YYYY-hh:mm-{2CH_LOCAL}",
  "store": "<notion|drive|git> (canonical; note if writable in place)",
  "index": "<slug>.index.md",
  "ops": "<slug>.ops.md",
  "readme": "proj.readme.md",
  "review_due": "DD-MM-YYYY",
  "staleness_days": 30,
  "automation_tier": "assisted|autonomous-capture|autonomous",
  "capture": "autonomous save-gate: each turn judge value/clarity/novelty, write/update in-scope durable facts unprompted; best-effort (chat-only); guardrails scope+privacy+conflict; reconcile on verify-on-use + on-contradiction, review_due sweep as backstop",
  "automation_rule": "write only via a connector that supports in-place update; use an external orchestrator for hands-off ingestion; do NOT depend on platform automations tied to a paid tier you may not keep"
}
```

> Duplicate this page per project, rename it, delete the HOW-TO banner below, and fill
> the placeholders. Setup walkthrough: proj.init.md. Behavior spec: proj.readme.md.

## HOW TO USE (delete this section after setup)

- What this is: one file = the project's durable memory. Shaped so an agent reads the top (HOT) to answer most questions, drills into a single record only when needed (WARM), and can drop the log (ARCHIVE) under context pressure.
- What the HOT sections are: STATE (below) is the file's live status - version, timestamps, where the master copy lives. DASH is the at-a-glance dashboard - overdue items, upcoming dates, running totals.
- Setup, 6 steps:
    1. Pick ONE canonical store (a Notion page, or a .md in Drive/repo). Record it in PROPS > canonical. Never edit two copies.
    2. Keep the domain-agnostic sections as-is: PROPS (optional, may be folded into STATE), CONVENTIONS, PARSE & PRUNE, INDEX, DASH, REF, MAP, GAP, LOG.
        - PROPS duplicates a few STATE fields (version/updated/canonical) for readability. STATE is the single authoritative volatile block - you may fold these fields into STATE instead and skip the PROPS section entirely, as the worked example (bills.md) does.
    3. Replace the example [CAT*] sections with your project's real categories. Update the INDEX to match (with counts).
    4. Give every trackable item a stable ID of the form CAT-KEY (e.g. SUB-NFLX, TASK-042, VENDOR-ACME, RISK-07). Never renumber; edits target the ID line.
    5. Set units/currency, date format, entity short-codes, and masking in PROPS/CONVENTIONS.
    6. If you need cross-links, use *Xn markers that point to entries in [REF].
- Update rule (every change): targeted search-and-replace on the record's ID line; append one line to [LOG]; bump STATE version + updated. Do NOT rewrite the whole file.
- Golden rules: one canonical copy; stable IDs; append-don't-drop; put anything unverified in [GAP]; keep it one-line-per-record and abbreviation-friendly; timestamps in the CONVENTIONS format.

## PROPS

- (optional section - see HOW TO USE)
- version: 0
- updated: DD-MM-YYYY-hh:mm-{2CH_LOCAL}
- canonical: <where the single true copy lives - e.g. this Notion page / Drive path / repo file>
- units: <currency or measurement rule - e.g. "USD; show original then conversion in ()">
- entities: <short codes for recurring people/teams/systems - e.g. A = ..., B = ...>
- id_mask: <how sensitive identifiers are masked - e.g. IDs shown ...NNNN, full in source; unmask on request>
- record_ids: stable keys per item (CAT-KEY). Edit by targeted replace on the ID line; never renumber.

## CONVENTIONS (binding rules)

- Time: DD-MM-YYYY-hh:mm-{2CH_LOCAL} (e.g. 16-07-2026-23:27-IL). Plain dates: DD-MM-YYYY. (Adjust format to taste, but keep it consistent.)
- Store: <plain .md / Notion blocks>. Note language / RTL handling if relevant (dir=rtl export on request).
- Units/currency: <the rule from PROPS, stated in full>. Include tax/fees basis if money is involved.
- Attribution/split: <how shared or co-owned items are divided or attributed; state the default and how each record shows it>.
- Refs: *Xn points to an entry in [REF].
- Purpose: <one line: what this project memory manages and how it is kept current>.
- Discipline: append + consolidate, never silently drop; move unverified claims to [GAP]; one canonical copy only.
- Confidence tag (optional): a record whose only source is weak (screenshot/OCR, verbal, estimate, single unconfirmed source) may carry a trailing [~ <reason, max ~6 words>, DD-MM-YYYY] tag on its record line. Default-absent = normal/verified confidence. One tag per line, appended last.

## PARSE & PRUNE (how an agent should read/edit this file)

- Tiers: HOT = STATE/PROPS/CONVENTIONS/INDEX/DASH (always read). WARM = the [CAT*] category sections (read on demand by ID). ARCHIVE = LOG (prunable; safe to drop under context pressure).
- Each WARM record is one self-contained line: `ID Name: facts. dates. owner/state. refs. [~ optional confidence tag]`.
- To answer "what's next / what changed / status of X": read DASH first, then the specific record by ID.

## INDEX (curated - one line each; keep counts current as it grows)

- [DASH] Dashboard: next actions, overdue, priorities, totals. READ FIRST.
- [CAT1] <Category 1 - short description> (n).
- [CAT2] <Category 2 - short description> (n).
- [CAT3] <Category 3 - short description> (n).
- [REF] Reference / rules / benefits cross-linked by *Xn.
- [MAP] Relationships / ownership / accounts map.
- [GAP] Open questions / unverified data.
- [LOG] Change log (ARCHIVE, prunable).

## [DASH] DASHBOARD

Overdue / needs attention now:

- <record ID - what, and since when>

Rolling actions (date order):

- DD-MM-YYYY <event / deadline - record ID>

Priorities / opportunities (verify before relying):

- <high-value pending items, cross-ref *Xn / IDs>

Totals / rollups (keep current):

- <key aggregate numbers - e.g. recurring monthly total, outstanding total, count by state>

## [CAT1] <CATEGORY 1 TITLE> (one line per tracked item in this category)

<!-- Example record schema - one line per item; delete this comment and the example. -->

- CAT1-KEY1 <Name>: <key facts>. <relevant dates>. <owner / status / amount>. <*Xn refs>.
- CAT1-KEY2 <Name>: ...
- CAT1-KEY3 <Name>: <key facts, weakly sourced>. <relevant dates>. <owner / status / amount>. [~ <short reason, max ~6 words>, DD-MM-YYYY].

## [CAT2] <CATEGORY 2 TITLE> (one line per tracked item in this category)

- CAT2-KEY1 <Name>: ...
- CAT2-KEY2 <Name>: ...

## [CAT3] <CATEGORY 3 TITLE> - PLACEHOLDER

<Fields to fill on next scan: list the expected attributes so the gap is obvious.>

- field a: TBD
- field b: TBD

## [REF] REFERENCE / RULES (cross-linked by *Xn) (background rules or benefits records point back to)

Last reviewed: DD-MM-YYYY-hh:mm-{2CH_LOCAL}. Staleness rule: if this timestamp is older than <N> days, suggest a refresh at the next relevant turn.

- *X1 <rule / benefit / reference>: <what it is; which records it applies to; source + caveat>.
- *X2 <...>: <...>.

## [MAP] RELATIONSHIPS / OWNERSHIP / ACCOUNTS (who pays/owns/handles which records)

- <who or what pays / owns / handles which records; payment methods; account owners; dependencies>

## [GAP] OPEN QUESTIONS (unconfirmed - do not treat as fact)

- <assumptions made, ambiguous values, missing documents, things to verify>

## [LOG] CHANGE LOG (ARCHIVE - prunable, safe to drop under context pressure)

- DD-MM-YYYY-hh:mm-{2CH_LOCAL}: <what changed / what source was scanned>. Keep newest on top; condense old entries rather than deleting.

---

TEMPLATE NOTES (delete): example IDs use CAT-KEY; example refs use *Xn; example placeholders use <...> and "TBD". Sections in ALL CAPS with [CODE] tags are the fixed skeleton; only the [CAT*] bodies and the angle-bracket placeholders are meant to be replaced.

## MAINTENANCE & AUTOMATION (runbook - keep here, or fold the essentials into STATE and point to proj.readme.md, as bills.md does)

- Autonomous capture (save-gate) + full operating guide (keywords/tasks) + instruction interface: see proj.readme.md (master spec, SHARED). It writes durable in-scope facts without being asked; limits and the backstop reconcile pass are specified there.
- Update loop (per change): 1) read <slug>.index.md; 2) fetch only the matching record; 3) edit that record's ID line in place; 4) append one [LOG] line; 5) bump STATE.version + STATE.updated; 6) refresh the index only if a section/ID/flag changed.
- Rules: follow CONVENTIONS; never rewrite the whole file; never create a second copy; unverified -> [GAP].
- Reconcile cadence: verify-on-use (check a record's freshness the moment it is actually fetched) plus re-check-on-contradiction (a new fact disagreeing with a stored one triggers [GAP] escalation right away, no separate step). STATE.review_due / staleness_days stays as the backstop sweep, unchanged, for whatever neither path catches.
- Reconcile pass (run when the backstop sweep is due): dedup; fold [LOG] into records + condense; flag stale/conflicting entries for a human yes/no (autonomous conflict-resolution is unreliable); run the probe eval from <slug>.ops.md; set new STATE.review_due.
- Automation: prefer in-place-update connectors (e.g. Notion MCP) for writes; for hands-off ingestion use an external orchestrator (n8n/Zapier/Composio) on the platform's API so it survives plan downgrades; avoid automations locked to a paid tier. Blueprint lives in README.md, Automation section.

## BOOTSTRAP (setup guidance - safe to drop once the file is running, as bills.md does)

- Setting this template up for a new project (or migrating an existing one with scattered history): follow proj.init.md phases 0-12 (its PHASE 12 is the portable kickoff prompt), then paste the filled prompt from PHASE 10. Spec/behavior authority stays proj.readme.md.
