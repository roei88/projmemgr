# proj.init.md

> This is the step-by-step bootstrap guide for setting up projmemgr on a project that
> doesn't have a memory context yet, whether brand-new or with history scattered across
> old chats and files - read by whoever, human or agent, is setting the project up.
> Follow the phases in order; the output is three new files (the project's main data
> file, its index, and its ops file) plus one prompt pasted into the project's
> instructions. For what the system does once it is running, see proj.readme.md.

## STATE

This file's own version-tracking block - the same STATE pattern every project file
uses, defined in full in proj.readme.md section 3 - tracks only this guide's version,
not any project's data:

```json
{"file": "proj.init.md", "role": "bootstrap/onboarding guide for new project contexts", "version": "0.1.0-alpha", "updated": "00-00-0000-00:00-IL", "spec": "proj.readme.md (shared; do not fork)", "templates": ["proj.md", "proj.index.md", "proj.ops.md", "proj.readme.md"]}
```

LINKS (core triad + ops): spec/readme = proj.readme.md | template main = proj.md | template index = proj.index.md | template ops = proj.ops.md

## PHASE 0 - PRECONDITIONS (do not skip)

- 0.1 Pick a STORE that supports in-place update via a connector (Notion is the reference; some first-party connectors are READ-ONLY - they cannot update, so they fail this system). Verify by doing one test edit before investing.
- 0.2 Confirm the connector is enabled in the project/chat that will use it.
- 0.3 Pick a short project SLUG (lowercase, no spaces), e.g. bills, health, car, work. All file names derive from it.
- 0.4 Decide DOMAIN SCOPE in one sentence: what belongs in this memory and what does not.

## PHASE 1 - NAMING & ID SCHEME

- 1.1 File names (same naming rule as proj.readme.md section 1): <slug>.md (canonical), <slug>.index.md (index), <slug>.ops.md (ops), plus the SHARED proj.readme.md (spec, one per workspace - never fork it) and proj.init.md (this guide). A leading dot or underscore is cosmetic sorting only, not part of the rule.
- 1.2 ID scheme = CAT-KEY. Choose 4-8 CATEGORY prefixes that cover the domain and will not change (illustrative only, replace with your own: for a job-search project JOB- CO- CV- IVW-; for a home project ROOM- APPL- WARR- VEND-). Rules: uppercase prefix, stable key after the dash, never renumber, one record = one id line.
- 1.3 Write the prefix list down now - it goes into the index, the readme instance section, and the paste prompt.

## WRITE-VERIFICATION RULE (applies to EVERY write in every phase below)

Never trust a write until you have confirmed it landed.

1. A write is not done until it is verified. A connector "success" response is NOT proof: a multi-operation edit can return success while silently skipping the operations whose text did not match.
2. Verify by re-fetching the page, or by re-running the same old_str as a SINGLE operation.
3. Read that result carefully: "No matches found" proves the edit already landed ONLY if the old_str was short and unique - with a long string it usually means the stored text differs (separators, double spaces, auto-links, escaping), so the edit may never have happened. "Multiple matches" means accidental duplication.
4. Therefore: prefer short, targeted edits - one concept per operation - and verify them one by one, instead of one large batch you cannot audit.
5. Report exactly what failed. Never present an unverified write as done. Full detail and store-specific quirks: the ops file, section 4.
6. Portability: points 1-5 hold for any store; store-specific quirks live in the ops file, section 4.

## PHASE 2 - CREATE THE CANONICAL FILE (<slug>.md)

- 2.1 Duplicate proj.md (template) or create fresh with this skeleton, in order: LINKS line -> STATE (fenced json) -> PROPS (optional readable duplicate of a few STATE fields) -> CONVENTIONS -> DASH (optional at-a-glance dashboard) -> record sections by category -> [GAP] (unverified facts, pending confirmation) -> [LOG] (timestamped change history).
- 2.2 Fill STATE keys: file, version (start 1), updated (DD-MM-YYYY-hh:mm-{2CH_LOCAL}), store, index, ops, readme, review_due (+30d), staleness_days (30), automation_tier (one of: assisted | autonomous-capture | autonomous; start at autonomous-capture), capture (save-gate description), automation_rule. Set "readme": "proj.readme.md".
- 2.3 Fill CONVENTIONS for this domain: time format, currency/units rule, split/attribution rule, masking rule, ref marker (*Xn), discipline (append+consolidate, unverified -> [GAP], one canonical copy).
- 2.4 Tiering: mark HOT (STATE/PROPS/CONVENTIONS/INDEX/DASH), WARM (record sections), ARCHIVE ([LOG], prunable).

## PHASE 3 - SEED THE DATA

### 3A. New project (no history)

- 3A.1 Create only the record sections you actually need; leave the rest out.
- 3A.2 Seed each known item as one dense line: `CAT-KEY | field: value | field: value | source: <where it came from>`.
- 3A.3 Anything unverified goes to [GAP], never into a record.

### 3B. Existing project (history exists)

- 3B.1 Inventory sources: past chats, uploaded files, bills/PDFs, spreadsheets, emails. List them before extracting.
- 3B.2 Extract facts source by source (OCR if needed). Distill SEMANTIC facts ("pays X monthly with card ...NNNN"), not episodic chatter ("we discussed X on Tuesday").
- 3B.3 Assign a CAT-KEY id to each fact as you go; if two sources disagree, keep the newer one AND log the conflict in [GAP] for a human yes/no. Never silently pick.
- 3B.4 Dedupe before writing: search by id/account/name. Same thing under two names = one record.
- 3B.5 Record provenance on each line (source + date) so a future conflict is resolvable. If that provenance is inherently weak (screenshot/OCR, hearsay, estimate), set a `[~ <short reason, max ~6 words>, DD-MM-YYYY]` tag on the same line instead of also opening a duplicate [GAP] bullet for the same fact.
- 3B.6 Freeze the old material as read-only backup; do not keep two live copies.

## PHASE 4 - CREATE THE INDEX (<slug>.index.md)

- 4.1 Purpose: pointers + coarse flags ONLY. No figures, no amounts, no dates that change.
- 4.2 Contents: one line per section (what it holds, when to open it), the CAT-KEY registry (prefix -> what it covers), coarse state flags (e.g. "has open items: yes"), STATE pointer, LINKS line.
- 4.3 Rule: index changes ONLY on a section/id/flag change - never on a value edit.

## PHASE 5 - CREATE THE OPS FILE (<slug>.ops.md)

- 5.1 Section 1: runbook pointer (the loop lives in the canonical file / readme; do not duplicate it).
- 5.2 Section 2: PROBE/EVAL set - aim for about one per category; a handful is plenty (the bills example ships 7). Build them from facts you know, each targeting a different record. Format: `Pnn <question> -> <expected answer>. [RECORD-ID]`. Score COVERAGE (right record fetched) and ACCURACY (right answer) separately. Target coverage >= 90%.
- 5.3 Section 3: automation state (optional future): trigger -> extract/OCR -> LLM-structure -> dedupe by id -> update-in-place-or-create -> [LOG] -> review gate. Use an external orchestrator on the store API so it survives plan downgrades.
- 5.4 Set STATE: probe_items, last_probe_run (null), automation_status ("blueprint only").

## PHASE 6 - WIRE THE README (proj.readme.md)

- 6.1 proj.readme.md is SHARED and already exists in this workspace - do NOT copy or fork it. Add a dedicated instance part under its section 10: `### <SLUG> INSTANCE`, listing files, id prefixes, capture triggers, cadence (staleness/review_due), domain-specific conventions, store constraints, and the filled paste prompt.
- 6.2 Only if you are bootstrapping a workspace that has no readme at all: create proj.readme.md by copying this reference spec's behavior (sections 0-9, 11, 12 and the CORE INSTRUCTION) as-is; under section 10 keep only the "Generic (template default)" part and add your own `### <SLUG> INSTANCE` - do NOT copy the BILLS INSTANCE block (it is this repo's demo), and point STATE.applies_to at your own files. Then do 6.1. Never create a second readme alongside an existing one.
- 6.3 The readme is authoritative for behavior; the canonical file's runbook points at it rather than restating it.

## PHASE 7 - THREE-DIRECTIONAL LINKS (must)

- 7.1 Add a LINKS line at the TOP of each of the three core files so any one reaches the other two (plus ops):
    - <slug>.md -> index + readme (+ops)
    - <slug>.index.md -> canonical + readme (+ops)
    - proj.readme.md -> canonical + index (+ops), per instance
- 7.2 Verify by opening each file and confirming the other two are named. A broken triangle = an agent that guesses.

## PHASE 8 - PASTE THE PROMPT INTO PROJECT INSTRUCTIONS

- 8.1 Take the prompt in PHASE 10 below (also in the Prompts section of README.md), fill the slots, paste into the project's custom instructions.
- 8.2 Keep it short. Longer instructions lower adherence; only load-bearing rules belong there. Everything else lives in the readme.
- 8.3 Store the exact same filled text in the readme instance part, so file and instruction never diverge.

## PHASE 9 - FIRST-RUN VALIDATION (prove it works before trusting it)

- 9.1 Read test: ask a which/where/exists question -> it must be answered from the index alone, without opening the canonical file.
- 9.2 Fetch test: ask a value question -> exactly one record fetched, correct answer.
- 9.3 Write test: state a small durable change -> it must update the right id line in place, append one [LOG] line, bump STATE, and confirm in one line. Then open the file and confirm no duplicate section and no whole-file rewrite.
- 9.4 Guardrail test: state something out-of-scope (unrelated personal chatter) -> it must NOT be stored. State something ambiguous -> it must ask or park it in [GAP].
- 9.5 Privacy test: mention a secret-like value -> it must be masked or stored as a reference, never in full.
- 9.6 Probe test: run 3 items from the ops probe set and record the result.
- 9.7 Only after 9.1-9.6 pass, treat the context as live.

## PHASE 10 - THE CLEAN PASTE PROMPT (final deliverable)

Fill {SLOTS} and paste into the project instructions. Nothing else is required for day-to-day use.

The paste-ready generic instruction is maintained in ONE place - the Prompts section of README.md (single source of truth). Copy it from there, fill the slots, and paste. Do not keep a second copy of the block in this guide - that duplication is exactly what the system exists to prevent.

Slot key: {STORE} = notion/other; {CANONICAL} = <slug>.md; {INDEX} = <slug>.index.md; {OPS} = <slug>.ops.md; {DOMAIN_LIST} = the scope sentence from 0.4 as a short list; {PREFIXES} = the id prefixes from 1.2.

## PHASE 11 - AFTER GO-LIVE

- 11.1 Cadence: two triggers do most of the work, and neither costs anything until it fires. Verify-on-use: when a record is actually fetched to answer something, check it right there - re-confirm it if the source is at hand, otherwise flag it in [GAP] rather than trust a stale value. Re-check-on-contradiction: a new fact that disagrees with a stored one (per 3B.3) means log the conflict in [GAP] immediately, independent of any date. Backstop: when past STATE.review_due, run the reconcile sweep (dedupe, fold [LOG] into records, flag conflicts for human yes/no, run the probe set, set a new review_due, bump STATE) to catch anything the two triggers above never touched.
- 11.2 Honest limits to keep in mind: the backstop sweep is the safety net, not optional - a record that is never fetched and never contradicted only gets caught there. Full hands-off ingestion requires the PHASE 5.3 automation.
- 11.3 Adding a new project later: repeat PHASE 0-10 with a new slug; reuse proj.readme.md and add a new instance part. Never fork the readme.

## CHECKLIST (copy when running this)

- [ ] 0 store verified writable + connector on + slug + scope sentence
- [ ] 1 id prefixes fixed
- [ ] 2 canonical file created: LINKS -> STATE -> PROPS -> CONVENTIONS at top (per 2.1)
- [ ] 3 data seeded (or migrated + deduped + provenance)
- [ ] 4 index created (pointers only)
- [ ] 5 ops created (probe set + automation state)
- [ ] 6 readme instance part added
- [ ] (ongoing, applies to every write above) every write verified one by one (Write-Verification Rule); nothing reported as done unverified
- [ ] 7 three-directional links verified
- [ ] 8 prompt filled + pasted
- [ ] 9 first-run validation 9.1-9.6 passed
- [ ] 11 review_due set as backstop; verify-on-use + re-check-on-contradiction understood as the primary cadence (Phase 10's paste prompt is covered by item 8 above)

## PHASE 12 - KICKOFF PROMPT (send this in ANY project, at any stage, to start or repair its context)

Purpose: one portable message the user can paste into a fresh or existing project. It points the agent at this guide and makes it detect the current state first, so a half-built or already-built context is repaired rather than duplicated. The same text is in the Prompts section of README.md.

```
Set up (or repair) this project's memory context.

1. Read proj.init.md - the bootstrap guide - and proj.readme.md for behavior. Follow them; do not invent a different structure. If the connector is off or unreachable, say so and stop.
2. Detect the current state BEFORE writing anything: search the workspace for this project's canonical file, its index, its ops file, and an instance part in proj.readme.md section 10.
   - Nothing exists -> run PHASE 0-10 for a new context.
   - Something exists -> never create a second copy: audit what is there against the spec, list what is missing, wrong, or duplicated, and repair it in place.
3. Ask me only what you cannot infer - the slug, the one-sentence domain scope, and the CAT-KEY prefixes - and propose your best answer for each so I can just confirm or correct.
4. Show a short plan first (files to create or change, prefixes, what gets seeded, what stays open) and wait for my go before writing.
5. On approval, execute: create or repair the canonical file, index, and ops file; add this project's instance part to proj.readme.md section 10; wire the three-directional LINKS; seed only facts you can source - anything unverified goes to [GAP], never guessed; mask secrets.
6. Verify every write (the Write-Verification Rule). A success response is NOT proof - a multi-operation edit can silently skip parts. Re-fetch the page, or re-run the same edit as a single operation; treat "No matches found" as proof only when that string was short and unique, since with a long string it usually means the stored text differs. Prefer short targeted edits and verify one by one. Report anything that failed instead of claiming done.
7. Finish by printing (a) the filled PHASE 10 paste prompt for this project, and (b) the first-run validation checklist 9.1-9.6 so I can test it.

Rules throughout: one canonical copy; edit in place, never rewrite whole files; never duplicate a record; stay inside this project's domain; never store passwords or full ID/card numbers.
```

Notes: the prompt ends with the artifacts the user needs - the paste prompt and the test list. It is a one-off kickoff message, not the standing instruction; the standing instruction is the paste-ready prompt in the Prompts section of README.md (see PHASE 10).
