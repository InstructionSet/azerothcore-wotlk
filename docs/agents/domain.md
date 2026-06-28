# Domain Docs

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

## Before exploring, read these

- **`CONTEXT-MAP.md`** at the repo root — it lists the bounded contexts and points at each `CONTEXT.md`. Read the context(s) relevant to your topic.
- **`docs/adr/`** — read ADRs that touch the area you're about to work in. Also check for context-scoped ADRs under the relevant context directory.

If any of these files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The `/domain-modeling` skill creates them lazily when terms or decisions actually get resolved.

## File structure

Multi-context repo (`CONTEXT-MAP.md` at the root is the index):

```
/
├── CONTEXT-MAP.md                             ← index of all bounded contexts + relationships
├── docs/adr/                                  ← system-wide architectural decisions
└── src/server/game/
    └── Movement/
        ├── CONTEXT.md                         ← Movement Behaviour context
        ├── Spline/
        │   └── CONTEXT.md                     ← Pathfinding context
        └── Waypoints/
            └── CONTEXT.md                     ← Scripted Paths context
```

## Use the glossary's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in the relevant `CONTEXT.md`. Don't drift to synonyms the glossary explicitly avoids.

If the concept you need isn't in the glossary yet, that's a signal — either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).

## Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _Contradicts ADR-0007 — but worth reopening because…_
