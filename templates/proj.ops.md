# proj.ops.md

LINKS (core triad): full file = <slug>.md | index = <slug>.index.md | readme/spec = proj.readme.md (SHARED - never fork)

> Operations file for one project - holds the health-check questions (the probe set)
> and this project's automation deploy state. Opened
> only during the reconcile pass or when setting up automation, not needed day to day.

## STATE (this ops file's own version, probe-set size, and last probe run - not the project data)

```json
{"file": "<slug>.ops.md", "version": 0, "updated": "DD-MM-YYYY-hh:mm-{2CH_LOCAL}", "canonical_file": "<slug>.md v?", "readme": "proj.readme.md", "probe_items": 0, "last_probe_run": null, "automation_status": "blueprint only"}
```

## 1. RUNBOOK

The update loop + reconcile pass live in <slug>.md > MAINTENANCE & AUTOMATION. This page holds what that section refers to: the probe set (2) and automation deploy state (3). Cadence rules (verify-on-use, re-check-on-contradiction, backstop sweep): see proj.readme.md sections 5-7 - not restated here.

## 2. PROBE / EVAL SET (about one per category; a handful is plenty; run at each reconcile pass)

How to build (see proj.init.md PHASE 5 for the walkthrough): pick questions whose answers you know from the file and that each target a different record/section. Store the expected answer + the record ID. This is your drift detector.

How to run: ask each cold (index-first); score COVERAGE (right record fetched?) and ACCURACY (right answer?) separately; log pass/fail + tokens; target coverage >= 90%; if it drops, fix index labels, not data; update STATE.last_probe_run. Re-verify against the record before failing (data may have legitimately changed).

- P01 <question> -> <expected answer>. [<RECORD-ID>]
- P02 <question> -> <expected answer>. [<RECORD-ID>]
- ... (about one per category - a handful is plenty)

## 3. AUTOMATION STATE (hands-off ingestion deploy status)

Full automation walkthrough (orchestrator choice, data flow, deploy steps, dedup rule): see README.md, Automation section - not restated here.

Per-project, stays here: whether automation is deployed yet for this file, tracked in this file's own STATE.automation_status (json block above). When you deploy, set STATE.automation_status = "deployed" and <slug>.md STATE.automation_tier = autonomous.

## 4. WRITE-PATH CAUTION (store-specific detail - see Verify Write in proj.readme.md section 5)

Portability: the rule itself - verify after every write - is store-agnostic and always applies. The specifics below describe how the Notion connector behaves; revisit this section if the store ever changes.

- Notion silently converts file-like names into auto-links, so an old_str typed as plain text will not match the stored linked form. Always copy the exact stored text from a fetch of the page before building a search-replace.
- A "No matches found" error is NOT automatically proof that the edit already landed: with a long old_str it often means the stored text differs (separators, double spaces, auto-links, escaping). Treat it as proof only when the old_str was SHORT and unique. Default practice: make short, targeted edits, one concept per operation, and verify them one by one.
- A multi-operation update can return success while silently skipping the operations that did not match. NEVER treat a success response as proof. Verify by (a) fetching the page, or (b) re-running the same old_str as a SINGLE operation - reading the result per the bullet above ("No matches" proves it landed only when the string was short and unique; "multiple matches" proves accidental duplication).
