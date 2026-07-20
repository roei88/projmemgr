# bills.ops.md

LINKS (core triad + ops): full file = bills.md | index = bills.index.md | readme/spec = proj.readme.md (SHARED - never fork)

> Example ops file for the fictional bills project - shows the maintenance side: a
> health-check pass, and a plan for hooking up automatic imports. Opened only when
> that check is due or on use, not needed for everyday use.

## STATE (this ops file's own version and last probe run - not the project data)

```json
{"file": "bills.ops.md", "version": 1, "updated": "00-00-0000-00:00-IL", "canonical_file": "bills.md v1", "readme": "proj.readme.md", "probe_items": 7, "last_probe_run": null, "automation_status": "blueprint only"}
```

## 1. RUNBOOK

The update loop + reconcile pass live in bills.md > (STATE + CONVENTIONS) and proj.readme.md sections 5-7. This page holds the probe set (2) and this project's automation state (3).

## 2. PROBE / EVAL SET (run when due, or on use; score coverage + accuracy separately)

Each question below targets a different record on purpose, so a wrong answer points
straight at which record needs fixing:

- P01 When does the music trial end and what happens then? -> 12-08-2026, becomes billed. [SUB-MUSIC]
- P02 Which card pays the AI assistant? -> ...5678. [SUB-AITOOL / MAP]
- P03 How many open debts and what is the total? -> 2 items, 1,200. [DEBT]
- P04 Is home internet active right now? -> No, suspended until 01-10-2026. [TEL-NET]
- P05 Monthly rent and how it splits? -> 4,000, 2,000 each. [HOME-RENT]
- P06 Vehicle registration expiry? -> 01-12-2026. [VEH-CAR]
- P07 What is unresolved about municipal tax? -> placeholder, no current document. [GAP]

## 3. AUTOMATION STATE (hands-off ingestion deploy status)

Full automation walkthrough (orchestrator choice, data flow, deploy steps, dedup rule): see README.md, Automation section - not restated here.

Per-project, stays here: whether automation is deployed yet, tracked in this file's own STATE.automation_status (json block above). On deploy set STATE.automation_status = deployed and bills.md STATE.automation_tier = autonomous.

## 4. WRITE-PATH CAUTION (store-specific detail - see Verify Write in proj.readme.md section 5)

Portability: the rule itself - verify after every write - is store-agnostic and always applies (see Verify Write in proj.readme.md section 5). The specifics below describe this instance's store (Notion); revisit this section if the store ever changes.

- Notion silently converts file-like names into auto-links, so an old_str typed as plain text may not match the stored linked form. Copy the exact stored text from a fetch of the page before building a search-replace.
- "No matches found" is proof an edit already landed ONLY when the old_str was short and unique; with a long string it usually means the stored text differs (separators, double spaces, auto-links, escaping). "Multiple matches" means accidental duplication.
- A multi-operation update can return success while silently skipping the operations that did not match. Never treat a success response as proof: re-fetch the page, or re-run the same old_str as a single operation. Prefer short, targeted edits, one concept per operation, verified one by one.
