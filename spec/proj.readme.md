# proj.readme.md

## CORE INSTRUCTION - AUTONOMOUS SAVE POLICY (authoritative for behavior; sections below only elaborate it - if anything conflicts, this block wins)

(Uses STATE, CAT-KEY, [LOG], [GAP] - each defined in plain words in sections 1-4 below;
skim there first if any are unfamiliar.)

You maintain this project's memory as a SELF-UPDATING knowledgebase. The user does NOT need to say "save". On every turn, run the save-gate:

1. Detect candidate facts the turn establishes.
2. Judge each - VALUE (durable + decision-relevant to this project's domain, not one-off/ephemeral), CLARITY (unambiguous + complete enough to be a record), NOVELTY (new / an update to an existing record / already known).
3. Act - DISCARD chatter, hypotheticals, out-of-scope, unverified opinion. WRITE a new in-scope durable fact as a CAT-KEY record. UPDATE an existing record's id line in place when a fact changes. If important but unclear, incomplete, or conflicting -> ask one line or park in [GAP]. Never guess, never duplicate.
4. After any write/update - append one [LOG] line, bump STATE, confirm in one line. Several updates in the same turn share ONE [LOG] line and one STATE bump, not one per fact.

Guardrails (mandatory): stay in this project's domain (do NOT store unrelated personal life). Never auto-store secrets (passwords, full national ID, full card numbers) - mask or store only a reference. If a stated fact contradicts a stored source, flag for yes/no; do not silently overwrite - but the SAME source correcting a value it gave earlier is a normal update, not a conflict: edit in place and keep the old value in the [LOG] line. Never make a second copy or rewrite the whole file; refresh the index only on a section/id/flag change. If the connector is off or a write fails, say so and hold the change. A write counts as done ONLY after it is verified - a connector "success" response is not proof (Write-Verification Rule: ops file, section 4). When past STATE.review_due, run the probe + dedup pass in the ops file. Autonomous salience is best-effort - the reconcile pass is the safety net.

(A weakly-sourced fact that still clears the write bar is written WITH the confidence tag defined in section 2, CONVENTIONS - not also opened as a duplicate [GAP] entry.)

LINKS (core triad + ops): full file = <slug>.md | index = <slug>.index.md | ops = <slug>.ops.md  ;  readme/spec (this file; SHARED by all projects) = proj.readme.md | bootstrap how-to = proj.init.md. Per-project filled values live in section 10 - nothing project-specific belongs above it.

Paste-ready instruction - GENERIC interface (fill {slots}): maintained in ONE place, the Prompts section of README.md. Copy it from there and fill the slots. This file stays authoritative for BEHAVIOR (the CORE INSTRUCTION above); the paste block itself lives in the Prompts section of README.md.

Filled per-project versions of this prompt live in section 10 (INSTANCE NUANCES) - one place only, so they cannot drift.

> This is the shared spec file for projmemgr. Follow the CORE INSTRUCTION block above
> every turn; sections 10 onward hold each project's filled-in instance part.

## 0. QUICKSTART FOR AN AGENT (read first)

- The system is a small set of linked files. ONE canonical file holds the data + a STATE block; a tiny index routes to it; an ops file holds the probe eval + automation state; this readme holds the spec.
- To ANSWER: read the index; fetch one record from the canonical file only if a pointer matches. Answer which/where/exists from the index alone.
- To UPDATE (autonomous capture, see CORE INSTRUCTION): edit the matching record's ID line IN PLACE, append one [LOG] line, bump the STATE block. Never rewrite the whole file; never create a second copy; if unsure or a value conflicts, ASK or put it in [GAP].
- To MAINTAIN: re-verify a record when you use it and have reason to doubt it's still current, and immediately when a new fact contradicts a stored one - do not wait for a scheduled pass. As the backstop, when today is past STATE.review_due (or STATE.updated is older than staleness_days), run the RECONCILE PASS (dedup + fold log + probe eval; section 7).
- Definitions, conventions, and the exact operating keywords/tasks are below.

## 1. FILE SET & ROLES

- <slug>.md - canonical single source of truth (data + STATE). Has HOT/WARM/ARCHIVE tiers inside. ONE per project.
- <slug>.index.md - pointer/router. Pointers + coarse state flags ONLY, no figures. Loaded first.
- <slug>.ops.md - operations: probe/eval set + automation deploy state.
- proj.readme.md - THIS: full spec + operating guide + instruction interface. SHARED across projects (one per workspace); each project adds an instance part under section 10. Never fork or copy it.
- proj.init.md - bootstrap how-to: creating and wiring a context for a project that has none.
- NAMING RULE (one rule, used everywhere): pick a lowercase <slug> per project; files are <slug>.md, <slug>.index.md, <slug>.ops.md. A leading dot or underscore is cosmetic sorting only.
- Rule: exactly ONE canonical copy. Index/ops/readme reference it; they never duplicate its values.

## 2. CONVENTIONS (binding)

- Time: DD-MM-YYYY-hh:mm-{2CH_LOCAL} (a two-letter local country/timezone code, e.g.
  IL). Plain dates: DD-MM-YYYY.
- Store: named in STATE.store; note whether it is writable in place.
- Units/currency: original value first, then conversion in parentheses (or the project's own rule). Prices include tax unless stated.
- Attribution/split: how shared/co-owned records divide; state the default and show each share.
- Refs: *Xn points to an entry in a [REF] section.
- IDs: every record has a stable CAT-KEY (category-key). Never renumber. Edits target the ID line.
- Masking: sensitive identifiers masked (e.g. ...NNNN); full value only in source.
- Discipline: append + consolidate, never silently drop; unverified -> [GAP]; one canonical copy only.
- [GAP]: the section holding facts that could not be verified, or that conflict with a
  stored value - parked there for a human yes/no, never guessed into a record.
- Confidence tag (optional): a record whose only source is weak (screenshot/OCR, verbal, estimate, single unconfirmed source) may carry a trailing [~ <reason, max ~6 words>, DD-MM-YYYY] tag on its record line. Default-absent = normal/verified confidence. One tag per line, appended last.

## 3. STATE BLOCK (authoritative volatile block)

- A fenced json block near the top of the canonical file, directly below the LINKS line (PHASE 7.1). It is the authority for volatile fields (version, updated, review_due, staleness_days, tier, pointers). It supersedes any version/updated lines in prose.
- Update rule: on every change, bump version and set updated; set review_due on the reconcile pass (section 7). Edit only these keys.

## 4. TIERS (parse & prune)

- HOT = STATE/PROPS/CONVENTIONS/INDEX/DASH (always read). PROPS = an optional
  human-readable duplicate of a few STATE fields (version/updated/store); DASH = an
  optional at-a-glance dashboard section (next actions, overdue items, totals).
- WARM = the category record sections (read on demand by ID).
- ARCHIVE = LOG (prunable; safe to drop under context pressure).
- Limit: the [LOG] is prunable, not a permanent record.

## 5. OPERATING GUIDE (keyword-driven checklist - readable by anyone)

How to read: Settings = the guide + limits; Variables = knobs; Keywords = reusable operations (verbs); Tasks = routines that call them. `...` continues a long line.

```
*** Settings ***
Documentation    SELF-MAINTAINING MEMORY. Keep one canonical file correct and current with
...              minimal effort: read the index first, write durable facts back in place,
...              unprompted. LIMITS: chat-only, recognition-based, best-effort (a miss is
...              silent) -> Deploy Full Automation for silent arrivals. Therefore Run
...              Reconcile Pass is the SAFETY NET and is not optional.

*** Variables ***
${CANONICAL}          <slug>.md
${INDEX}              <slug>.index.md
${OPS}                <slug>.ops.md
${README}             proj.readme.md      # shared, not per-project
${STORE}              <notion|drive|git ; writable in place?>
${ID_SCHEME}          CAT-KEY
${STALENESS_DAYS}     30
${REVIEW_DUE}         DD-MM-YYYY
${TIER}               autonomous-capture   # assisted | autonomous-capture | autonomous  (the only valid values)
@{CAPTURE_TRIGGERS}   any durable, decision-relevant fact in the domain, whether or not the user asks to save it

*** Keywords ***
Read Index First
    [Documentation]    Load ${INDEX}; answer which/where/exists from it; reach into ${CANONICAL} only on a pointer match.
Locate Record
    [Documentation]    Map the topic to one ${ID_SCHEME} id via the index registry.
Fetch Record
    [Documentation]    Pull the smallest thing that answers the need: one record, scoped search by id/label. Whole file only if 3+ sections.
Update Record In Place
    [Documentation]    Search-replace ONLY that record's id line. Never rewrite the whole file; never duplicate.
Create Record
    [Documentation]    New item -> fresh ${ID_SCHEME} id under the right section, then a matching index pointer.
Append Log Line
    [Documentation]    One timestamped line to [LOG] (newest on top). Never delete history.
Bump State
    [Documentation]    Increment STATE.version; set STATE.updated; set review_due when due.
Refresh Index If Structure Changed
    [Documentation]    Touch ${INDEX} only if a section/id/coarse-flag changed. Value edits never touch it.
Guard Duplicates
    [Documentation]    Before Create, search by id/account; if it exists, Update instead.
Guard Uncertainty
    [Documentation]    Unsure or value conflicts -> ask or park in [GAP]. Never guess.
Guard Write Path
    [Documentation]    Connector off or write fails -> say so and HOLD the change; do not silently drop.
Verify Write
    [Documentation]    Run after EVERY write. A "success" response is not proof: a multi-op edit can
    ...                silently skip operations that did not match. Confirm by re-fetching the page, or
    ...                by re-running the same old_str as a SINGLE operation. Reading that result: "No
    ...                matches" proves the edit landed ONLY if the old_str was short and unique (with a
    ...                long string it usually means the stored text differs); "multiple matches" means
    ...                accidental duplication. Prefer short, targeted edits, one concept per operation.
    ...                Report what failed instead of claiming done. Detail: ${OPS} section 4.

*** Tasks ***
Answer A Query
    [Tags]    read    routine
    Read Index First
    Locate Record    ${topic}
    Run Keyword If    matched    Fetch Record
Capture A Durable Change From Chat
    [Documentation]    Fires UNPROMPTED on @{CAPTURE_TRIGGERS}. Update in place, log, confirm in one line.
    [Tags]    capture    routine    unprompted
    Guard Write Path
    Locate Record    ${stated_change}
    Guard Duplicates
    Guard Uncertainty
    Run Keyword If    exists    Update Record In Place    ELSE    Create Record
    Append Log Line
    Bump State
    Confirm In One Line
Run Reconcile Pass
    [Documentation]    SAFETY NET for capture misses + anything never mentioned. Runs past ${REVIEW_DUE} or when stale.
    [Tags]    reconcile    scheduled    not-optional
    Dedupe Records
    Fold Log Into Records
    Flag Conflicts For Human Yes No
    Run Probe Eval    ${OPS}
    Set Review Due    +${STALENESS_DAYS}d
    Bump State
Deploy Full Automation
    [Documentation]    OPTIONAL zero-touch ingestion for silent arrivals. External orchestrator on the store API. Steps in ${OPS}.
    [Tags]    optional    external
```

## 6. CAPTURE MODE (summary of the CORE INSTRUCTION; tier autonomous-capture) + LIMITS

- Trigger: a durable, in-scope fact appears in the turn - the user does NOT have to ask. Run the save-gate in the CORE INSTRUCTION (authoritative); this section only summarizes it.
- Guardrails: dedupe before create; ask/GAP on uncertainty or conflict; hold + report on write failure; one-line confirmation.
- Limits (honest): chat-only, recognition-based, best-effort. It does NOT replace source-triggered ingestion. Verify-on-use and re-check-on-contradiction narrow the gap turn to turn, but the review_due reconcile pass (section 7) is still mandatory as the backstop safety net.

## 7. RECONCILE PASS (when due or on use) & PROBE EVAL

- Cadence: the two live triggers are verify-on-use (re-check a record when you fetch it and doubt it's still current) and re-check-on-contradiction (a new fact disagrees with a stored one - flag for a human yes/no per section 2's Discipline bullet; do not silently overwrite). Neither waits for a scheduled pass.
- When (backstop sweep): past STATE.review_due, or STATE.updated older than staleness_days.
- Do: dedup; fold [LOG] into records + condense; flag stale/conflicting entries for a HUMAN yes/no (autonomous conflict-resolution is unreliable); run the probe set (score COVERAGE = right record fetched, and ACCURACY = right answer, separately); set new review_due; bump STATE. The probe set lives in <slug>.ops.md.

## 8. AUTOMATION (optional, full hands-off)

- Use an external orchestrator (n8n/Zapier/Composio) on the store's API so it survives plan downgrades; do NOT depend on platform automations tied to a paid tier you may drop. Full blueprint + deploy steps are in the Automation section of README.md; <slug>.ops.md tracks only this project's own deploy state. On deploy, set STATE.automation_tier=autonomous.

## 9. INSTRUCTION INTERFACE (paste into the project/profile instructions)

Generic interface - SINGLE SOURCE: the paste-ready generic prompt is maintained in ONE place, the Prompts section of README.md. Copy it from there and fill the slots; do not keep another copy here. This file stays authoritative for BEHAVIOR (the CORE INSTRUCTION at the top); the paste block lives in the Prompts section of README.md so it cannot drift.

Slot key: {STORE} = the store (e.g. notion); {CANONICAL} = <slug>.md; {INDEX} = <slug>.index.md; {OPS} = <slug>.ops.md; {DOMAIN_LIST} = the project's scope as a short list; {PREFIXES} = the project's CAT-KEY prefixes. (The block hardcodes "proj.readme.md" - there is no {README} slot to fill.) Filled per-project versions live in section 10.

## 10. INSTANCE NUANCES

### Generic (template default)

- Fill Variables in section 5 and the instruction {slots} in section 9 per project. Add project-specific rules here.

### BILLS INSTANCE (example - fictional data; see examples/bills)

- Files: {CANONICAL}=bills.md; {INDEX}=bills.index.md; {OPS}=bills.ops.md; {README}=proj.readme.md; {STORE}=notion.
- IDs: CAT-KEY prefixes SUB- TEL- HOME- VEH- INS- DEBT-. ([PAY] and [GAP]/[LOG] are map/utility sections, not CAT-KEY record categories.)
- Capture triggers: new/changed bill, price, card, date, status, fine.
- Cadence: staleness_days=30; review_due=31-08-2026; tier=autonomous-capture.
- Currency rule: original first, conversion in (). Split shared bills between Person A and Person B, default 50/50 unless stated (assumption - confirm).
- Masking: card numbers shown as ...NNNN; full value only in the source document.
- Filled instruction to paste:

```
Ongoing context (Notion, via connector) - keep it as a self-updating knowledgebase:
- Read bills.index.md first; fetch a record from bills.md only on a pointer match; answer which/where/exists from the index alone.
- Save on your own judgment (I should NOT have to say "save"): each turn, watch for durable, decision-relevant facts in this project's domain (bills, subscriptions, telecom, household, vehicle, insurance, fines, payment methods). On spotting one, persist unprompted: new -> create a CAT-KEY record (SUB-/TEL-/HOME-/VEH-/INS-/DEBT-); changed -> update that record's id line in place; then append one [LOG] line, bump STATE, confirm in one line.
- Per fact decide: DISCARD chatter/one-off/hypothetical/out-of-scope; WRITE/UPDATE durable in-scope facts; if important but unclear, incomplete, or conflicting -> ask one line or park in [GAP]. Never guess, never duplicate.
- Guardrails: stay in this project's domain (don't store unrelated personal life); never auto-store secrets (passwords, full ID/card numbers) - mask or store a reference; if a fact contradicts a stored source, flag for yes/no, don't overwrite.
- Never make a second copy or rewrite the whole file; refresh bills.index.md only on a section/id/flag change. If the connector is off or a write fails, say so and hold. Re-verify a record when you use it and it is past its freshness horizon, or when a new fact contradicts it; run the probe + dedup pass in bills.ops.md when past STATE.review_due. Full spec: proj.readme.md.
```

## 11. CHANGE DISCIPLINE

- Every change: edit in place -> refresh anything else that mirrors that fact (a status line in [DASH], a related record, the index's coarse flags) so nothing goes stale -> append one [LOG] line -> bump STATE -> VERIFY the write (see Verify Write in section 5; full detail in the ops file, section 4). Add new instance nuances under section 10 as a dedicated part. Keep this readme as the single spec; the canonical file's runbook may point here rather than duplicate it.

## STATE

This file's own version marker, bumped whenever this spec changes - not project data:

```json
{"file": "proj.readme.md", "role": "master spec (polymorphic: template + instance readmes)", "version": "0.1.0-alpha", "updated": "00-00-0000-00:00-IL", "bootstrap": "proj.init.md", "applies_to": ["proj.md", "proj.index.md", "proj.ops.md", "proj.init.md", "examples/bills/*"]}
```

## 12. BOOTSTRAP (creating a context for a project that has none)

- Step-by-step onboarding guide (new projects and existing projects with scattered history): proj.init.md - phases 0-12, first-run validation tests, the clean paste prompt with slot key (PHASE 10), and the portable kickoff prompt to send inside any project (PHASE 12).
- Division of labor: proj.init.md = HOW to set one up (its PHASE 12 holds the portable kickoff prompt to send inside any project, new or existing); this readme = WHAT the system is and HOW it behaves once live. Do not duplicate content between them.
