# proj.index.md

LINKS (core triad + ops): full file = <slug>.md | readme/spec = proj.readme.md (SHARED - never fork) | ops = <slug>.ops.md | bootstrap how-to = proj.init.md

> This file is a short pointer list (a router) for one project's memory file - it names
> each section and record without repeating the actual data. An AI assistant reads this
> first, then opens the full file only to fetch a matching record (this index/fetch
> split is projmemgr's progressive-disclosure pattern). Copy this template per project,
> rename it, and fill in the placeholders.

## HOW TO USE (delete after setup)

- Build it: 1) point at the full file (location + fetch method). 2) list a SECTION MAP: code -> what it holds -> when to fetch. 3) list a RECORD REGISTRY: stable ID -> 1-3 word semantic label. 4) keep coarse state flags (placeholder/inactive/suspended) but NO figures/dates/amounts.
- Keep in sync: since the index has no values, value edits to the full file don't touch it. Update only when sections, IDs, or state flags change; bump index version.
- Golden rules: pointers not data; semantic labels good enough to route on alone; answer existence/"where" from the index without fetching; pull the minimum; if no pointer matches, the full file likely doesn't cover it.

## FULL FILE (fetch target)

- name: <full file name> (its version lives in that file's own STATE block - never duplicated here)
- location: <Notion page id/url, or Drive path, or repo path>
- backup: <if any; mark do-not-edit>
- FETCH PROTOCOL: <how to pull one record - e.g. Notion search with page_url=<url> and query=<ID/label> for snippet-level; or open the file and jump to the ID line. Fetch the whole file only for 3+ sections.>
- INDEX SYNC: rebuild when section list / record IDs / state flags change; bump index version.

## HOW TO ROUTE

- Match query -> pointer -> fetch that one section/record. Answer existence/"which record/where" from this index alone.
- No match -> full file probably doesn't cover it; say so, don't fetch blindly.

## SECTION MAP (code -> what it holds -> when to fetch)

- [DASH] next actions / priorities / totals -> "what's next / status / totals".
- [CAT1] <what it holds> -> "<trigger words>".
- [CAT2] <what it holds> -> "<trigger words>".
- [REF] rules/refs (*Xn) -> "<trigger words>".
- [MAP] ownership/accounts -> "who owns/pays X".
- [GAP] open questions -> "unverified / assumption".
- [LOG] change history (ARCHIVE) -> rarely fetch.

## RECORD REGISTRY (prefix once; fetch by full ID)

- CAT1-: KEY1 <label> | KEY2 <label> | ...
- CAT2-: KEY1 <label> | KEY2 <label> | ...
- REF-: X1 <label> | X2 <label> | ...

<!-- add coarse state in parentheses only where it aids routing, e.g. KEY3 <label>(PLACEHOLDER) -->

## PROPS

- OPTIONAL: mirrors STATE below for readability; STATE is authoritative and these fields may be folded into it instead.
- index version: 0
- updated: DD-MM-YYYY-hh:mm-{2CH_LOCAL}
- points to: <full file name> (see that file's STATE block for its current version)
- rule: pointers + coarse state flags only, never figures. Full file is the single source of truth.

## STATE (this index's own version number and last-updated stamp - not the full file's data)

- index v0; points to <slug>.md (see its STATE block for the full file's version/updated).
- review_due: DD-MM-YYYY (staleness Nd). If past due, run the reconcile pass - see <slug>.ops.md.
- companions: <slug>.ops.md = maintenance runbook + probe/eval set + automation state; proj.readme.md = shared behavior spec; proj.init.md = bootstrap how-to.
- rule unchanged: pointers + coarse state flags only, never figures.
