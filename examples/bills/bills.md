# bills.md

LINKS (core triad + ops): index = bills.index.md | readme/spec = proj.readme.md (SHARED - never fork) | ops = bills.ops.md | bootstrap how-to = proj.init.md

> Worked example of a canonical memory file, built for a fictional household-expenses
> project. All values below are made up and exist only to show the file's shape - do
> not treat any figure, name, or number as real. It demonstrates the standard building
> blocks (a machine-read status block, one summary line per bill, stable record IDs,
> and a layered read-first/on-demand/archive structure), each explained where it first
> appears below.

## STATE (authoritative volatile block - edit only these keys)

This example's live status block - version, timestamps, where the real copy would
live:

```json
{
  "file": "bills.md",
  "version": 1,
  "updated": "00-00-0000-00:00-IL",
  "store": "notion (canonical, writable in place via update_page)",
  "index": "bills.index.md",
  "ops": "bills.ops.md",
  "readme": "proj.readme.md",
  "review_due": "31-08-2026",
  "staleness_days": 30,
  "automation_tier": "autonomous-capture",
  "capture": "autonomous save-gate: each turn judge value/clarity/novelty, write/update in-scope durable facts unprompted; best-effort (chat-only); guardrails scope+privacy+conflict; reconcile verify-on-use, re-check on contradiction",
  "automation_rule": "write via an in-place-update connector; use an external orchestrator for hands-off ingestion; do NOT depend on platform automations tied to a paid tier you may not keep"
}
```

## CONVENTIONS (binding)

- Time: DD-MM-YYYY-hh:mm-{2CH_LOCAL}. Currency: original first, conversion in ().
- Shared items split 50/50 between Person A and Person B unless stated.
- Card numbers masked as ...NNNN. Full values live only in the source document.
- Refs: *Xn points to an entry in [REF].

## PARSE & PRUNE

- Tiers: HOT = STATE/CONVENTIONS/INDEX/DASH (always read). WARM = SUB..MAP (read on demand by ID). ARCHIVE = LOG (prunable).
- Each WARM record is one self-contained line: `ID Name: facts. dates. card/method.`

## INDEX (curated - one line each per section)

- [DASH] Dashboard: next actions, overdue, monthly total. READ FIRST.
- [SUB] Digital subscriptions (3).
- [TEL] Phone and internet (2).
- [HOME] Rent and household (3).
- [VEH] Vehicle (1).
- [INS] Insurance (2).
- [DEBT] Fines / open debts (2).
- [REF] Reference / rules cross-linked by *Xn (3).
- [MAP] Card -> service map.
- [GAP] Open questions.
- [LOG] Change log (ARCHIVE, prunable).

## [DASH] DASHBOARD (what's overdue, coming up, and the running totals)

Overdue / needs attention now:

- DEBT-0002 Traffic fine 900: OVERDUE since 01-02-2026, escalating.

Rolling actions (date order):

- 12-08-2026 SUB-MUSIC trial ends -> then billed.
- 30-08-2026 DEBT-0001 parking fine 300 due.
- 01-09-2026 TEL-CELL plan change pending.
- 01-10-2026 TEL-NET home internet suspension ends.
- 01-12-2026 VEH-CAR registration expires.

Totals: recurring monthly ~ SUB 150.00 (120.00 while MUSIC in trial) + TEL 260.00 (60.00 while NET suspended) + HOME 4,500 (HOME-TAX placeholder, excluded) + INS 200 (INS-CAR 3,600/yr billed annually, excluded here). Outstanding fines: 1,200 (2 items).

## [SUB] SUBSCRIPTIONS (recurring digital services, one line each)

- SUB-STREAM Streaming Service: 50.00/mo. renews 23 monthly. card ...1234. active. *R1.
- SUB-AITOOL AI Assistant: USD 20/mo (70.00). renews 05 monthly. card ...5678. active.
- SUB-MUSIC Music Service: 30.00/mo. trial ends 12-08-2026 -> then billed. card ...1234. [~ screenshot-sourced date, 14-07-2026]

## [TEL] TELECOM (phone and internet plans, one line each)

- TEL-CELL Carrier A mobile: 60.00/mo. plan change pending 01-09-2026. card ...5678.
- TEL-NET Carrier B home internet: 200.00/mo. SUSPENDED until 01-10-2026.

## [HOME] HOUSEHOLD (rent and recurring household costs, one line each)

- HOME-RENT Rent: 4,000/mo. lease 01-07-2026 to 30-06-2027. split A 2,000 / B 2,000.
- HOME-UTIL Utilities: ~500/mo variable. billed to Person B account. split 50/50. *R3.
- HOME-TAX Municipal tax: PLACEHOLDER - no current document. see [GAP].

## [VEH] VEHICLE (registration and related dates, one line each)

- VEH-CAR Vehicle (plate 00-000-00): registration expires 01-12-2026. last test 15-12-2025.

## [INS] INSURANCE (active policies, one line each)

- INS-CAR Insurer X policy ...4321: 3,600/yr. renews 31-05-2027. *R2.
- INS-HEALTH Insurer Y policy ...8765: 200/mo. active.

## [DEBT] FINES / OPEN DEBTS (unpaid amounts, one line each)

- DEBT-0001 Parking fine: 300. due 30-08-2026. unpaid.
- DEBT-0002 Traffic fine: 900. OVERDUE since 01-02-2026. escalating.

## [REF] REFERENCE / RULES (cross-linked by *Xn) (fictional rules for this worked example)

Last reviewed: 00-00-0000.

- *R1 Streaming bundle rule: SUB-STREAM's plan includes a bundled second seat; if either household member cancels, the other reverts to the standalone rate.
- *R2 Insurer X renewal/cancellation rule: INS-CAR auto-renews unless cancelled 30 days before the renewal date; no partial-year refund on early cancellation.
- *R3 Utilities split exception: HOME-UTIL defaults to the 50/50 split, but reverts to metered actual usage if one party's usage exceeds baseline by more than 20% for two consecutive months.

## [MAP] PAYMENT METHODS MAP (which card pays which record above)

- card ...1234: SUB-STREAM, SUB-MUSIC.
- card ...5678: SUB-AITOOL, TEL-CELL.

## [GAP] OPEN QUESTIONS (unconfirmed - do not treat as fact)

- HOME-TAX: no current bill on file; last known document is stale. Verify before use.

## [LOG] CHANGE LOG (ARCHIVE - prunable)

- 23-07-2026-00:00-IL: renamed [PAY] to [MAP] (same concept, aligned to template naming) and added [REF] with *R1-*R3 fictional cross-referenced rules; updated CONVENTIONS and PARSE & PRUNE to match.
- 19-07-2026-09:00-IL: folded the SUB-MUSIC screenshot caveat into the record as a confidence tag; removed the duplicate [GAP] entry.
- 18-07-2026-20:40-IL: seeded example records from dummy sources.
