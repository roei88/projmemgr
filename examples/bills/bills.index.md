# bills.index.md

LINKS (core triad + ops): full file = bills.md | readme/spec = proj.readme.md (SHARED - never fork) | ops = bills.ops.md | bootstrap how-to = proj.init.md

> Example pointer file (a router) for the fictional bills project - lists where to
> look, not the actual numbers. Load this first; fetch one record from bills.md only
> when a pointer matches.

## SECTION MAP (code -> what it holds -> when to fetch)

- [DASH] next actions / overdue / totals -> "what is due, monthly total"
- [SUB]  digital subscriptions           -> "streaming, AI tool, renewal, cancel"
- [TEL]  phone and internet              -> "carrier, data plan, router"
- [HOME] rent and household              -> "rent, utilities, municipal tax"
- [VEH]  vehicle                         -> "car, test, registration"
- [INS]  insurance                       -> "policy, renewal, coverage"
- [DEBT] fines and open debts            -> "fine, ticket, collection"
- [REF]  reference / rules (*Xn)         -> "rule, policy, cross-reference"
- [MAP]  card -> service map             -> "which card pays X"
- [GAP]  open questions                  -> "unverified / assumption"
- [LOG]  change history (ARCHIVE)        -> rarely fetch

## RECORD REGISTRY (prefix once; fetch by full ID)

- SUB-:  STREAM streaming | AITOOL ai-assistant | MUSIC music
- TEL-:  CELL mobile | NET home-internet (SUSPENDED)
- HOME-: RENT rent | UTIL utilities | TAX municipal-tax (PLACEHOLDER)
- VEH-:  CAR vehicle
- INS-:  CAR vehicle-policy | HEALTH health-policy
- DEBT-: 0001 (due) | 0002 (OVERDUE)
- REF-:  R1 streaming-bundle | R2 insurer-renewal | R3 utilities-split

## STATE (this index's own version and review-due date)

- index v1; points to bills.md (see its STATE block for the full file's version/updated).
- review_due: 31-08-2026 (staleness 30d). If past due, run the reconcile pass (verify-on-use, re-check on contradiction) - see bills.ops.md.
- rule: pointers + coarse state flags only, never figures. Full file is the single source of truth.
