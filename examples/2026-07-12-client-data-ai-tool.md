# Decision: Can this client data enter an AI tool?
**Made for:** anyone at the firm, day to day · **Reversibility:** reversible per-task, but data that has already crossed can't be un-shared — so branches err protective
**Mapped:** 2026-07-12 · **Status:** 2 open gaps

A worked example of the skill's output. The scenario is generic (a services firm handling client documents and financials); the open gaps are left honestly open because that's the point — they're the reading list.

```mermaid
flowchart TD
    A{"Contains identifiers?<br/>(SSNs, account numbers, tax IDs)"} -- yes --> R1["Strip identifiers first.<br/>They never enter any AI tool,<br/>whatever the plan tier"]
    A -- no --> B{"Public record, or written to be published?<br/>(marketing copy, listings, public filings)"}
    B -- yes --> OK1["USE FREELY.<br/>No confidentiality question exists"]
    B -- no --> C{"Another party's confidential business data?<br/>(a client's client, a tenant, a counterparty)"}
    C -- yes --> R2["Derived metrics only, via a local<br/>deterministic transform.<br/>Raw source stays out of the AI"]
    C -- no --> D{"Your own company financials?<br/>(GL, receivables, payroll-adjacent)"}
    D -- yes --> R3["Coded or derived output only.<br/>Raw never crosses; the mapping key<br/>stays with the raw data"]
    D -- no --> E{"Full-text document work?<br/>(contracts, agreements, plans)"}
    E -- no --> MISC["Everyday drafting and internal ops: use it.<br/>Least-sensitive form that does the job"]
    E -- yes --> G{{"OPEN GAP 1:<br/>Is the account on enterprise terms with<br/>a no-training default and retention controls?"}}
    G -- "no / unknown" --> HOLD1["HOLD - verify the plan tier first.<br/>One admin-console look"]
    G -- yes --> H{{"OPEN GAP 2:<br/>Do your client agreements permit<br/>service-provider processing?"}}
    H -- "no / unknown" --> HOLD2["HOLD - read the agreements first"]
    H -- yes --> OK2["USE IT - scoped folder, redact the<br/>radioactive parts, per your written policy"]

    style G fill:#fdf6e3,stroke:#b58900
    style H fill:#fdf6e3,stroke:#b58900
    style R1 fill:#fff0f0,stroke:#cc4444
    style OK1 fill:#eef7ee,stroke:#44aa44
    style OK2 fill:#eef7ee,stroke:#44aa44
    style MISC fill:#eef7ee,stroke:#44aa44
```

## Open gaps

| # | Node | What would answer it | Where |
|---|---|---|---|
| 1 | Enterprise terms + no-training default | One admin-console look at what plan the accounts are actually on, then the vendor's commercial terms | Vendor admin console + terms page |
| 2 | Client-agreement confidentiality clauses | Reading the actual agreements: do they permit service-provider processing, or require consent or carve-outs? | Your engagement letters / MSAs / operating agreements |

## Notes

- The easy cases are genuinely easy: anything written to be published exits at question two without touching a gap. If your tree can't produce a fast answer for the easy case, the branches aren't observable enough yet.
- Closing the gaps doesn't open the floodgates: company financials stay at coded-or-derived even on fully verified enterprise terms. The gaps only gate the full-text document lane.
- What this tree deliberately doesn't decide: which vendor. That's a different tree.
