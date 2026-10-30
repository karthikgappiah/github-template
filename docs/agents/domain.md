# Domain docs

This file tells the engineering skills how to use the domain docs of this repo.

## Read before you explore

1. Read `CONTEXT.md` at the repo root. If `CONTEXT-MAP.md` is at the root, read it. Then read each applicable `CONTEXT.md` that it gives.
2. Read the ADRs in `docs/adr/` that apply to your work area. In a multi-context repo, also read `src/<context>/docs/adr/`.

If a file does not exist, continue. Do not report the missing file. Do not recommend that a person creates it. `/domain-modeling` creates these files when a term or a decision is resolved. `/grill-with-docs` and `/improve-codebase-architecture` use `/domain-modeling`.

## Use the glossary terms

Use the `CONTEXT.md` terms when your output gives a domain concept. This rule applies to issue titles, refactor proposals, hypotheses, and test names. Do not use synonyms that the glossary prohibits.

If the glossary does not have the concept, do one of these steps:

- If the project does not use your term, change the term.
- If the glossary has a real gap, record it for `/domain-modeling`.

## Report ADR conflicts

If your output contradicts an ADR, tell the reader. Do not override the ADR silently. Use this format:

> _Contradicts ADR-0007 (event-sourced orders), but worth reopening because…_

## File structure

This repo has one context:

```
/
├── CONTEXT.md
├── docs/adr/
│   ├── 0001-event-sourced-orders.md
│   └── 0002-postgres-for-write-model.md
└── src/
```

A repo with `CONTEXT-MAP.md` at the root has multiple contexts:

```
/
├── CONTEXT-MAP.md
├── docs/adr/                          ← system-wide decisions
└── src/
    ├── ordering/
    │   ├── CONTEXT.md
    │   └── docs/adr/                  ← context-specific decisions
    └── billing/
        ├── CONTEXT.md
        └── docs/adr/
```
