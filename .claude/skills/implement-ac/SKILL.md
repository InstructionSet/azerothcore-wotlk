---
name: implement-ac
description: Implement AzerothCore work from a PRD or issue — build, lint, evidence, review.
disable-model-invocation: true
---

Implement the work described in the PRD or issue. Follow all conventions in `CLAUDE.md`.

## 1. Plan

Read the issue or PRD. Identify which **lanes** are active:

- **C++ lane** — source changes under `src/` or `modules/`
- **SQL lane** — data changes written to `data/sql/updates/pending_db_*/`

Both lanes can be active in one run.

**Done when**: lanes identified, issue understood.

## 2. Build loop

```
write code → cmake --build D:/Data/AC/build -j<cores> → fix errors → repeat
```

Builds are incremental — only changed files recompile. Keep increments small.

**Done when**: `cmake --build` exits 0 with no errors.

## 3. Unit test opportunity

Check once, after the first green build:

- **Pure utility** (no `Player*`, `Creature*`, `Map*`, or live-system dependencies) → consider adding a Google Test. Register via `ACORE_MODULE_TEST_SOURCES` in the module's `CMakeLists.txt`; follow patterns in `src/test/`.
- **Game-state-dependent** → skip. Do not attempt to unit-test live-system code.

**Done when**: test added, or skip noted with reason.

## 4. Gates

Run all applicable gates. All must exit 0 before continuing.

| Gate | When |
|---|---|
| `python apps/codestyle/codestyle-cpp.py` | C++ lane |
| `python apps/codestyle/codestyle-sql.py` | SQL lane |
| `ctest` | Existing tests cover the changed area |

**Done when**: all applicable gates exit 0.

## 5. Evidence

Gather the strongest available **evidence** that the change works. Use the AzerothCore MCP. This step is **advisory** — it informs `/review`, it is not a gate.

Evidence hierarchy (strongest first):

1. **DB assertion** — `mcp_sandboxmcp_query_world` or `mcp_sandboxmcp_query_characters` returns the expected state. SQL lane changes always produce this; always try it.
2. **Log assertion** — `mcp_sandboxmcp_search_log` or `mcp_sandboxmcp_read_log_tail` finds the expected line.
3. **Command output** — `mcp_sandboxmcp_execute_command` returns expected response.
4. **None** — behavior is stochastic or unobservable. Note explicitly: "no deterministic evidence available."

Document what was checked, what was found, and the evidence tier.

**Done when**: evidence documented (even if tier 4).

## 6. Commit

Commit to the current feature branch. Do not push.

Commit message must state: what changed (C++ / SQL / both) and the evidence tier gathered.

**Done when**: committed to feature branch.

## 7. Review

Run `/review`. Pass two inputs:

1. The issue or PRD as the spec source (required — so the Spec axis can fire).
2. The evidence document from step 5 as supplementary context for the Spec sub-agent: what was verified, at what tier, and what was unverifiable.

The Spec sub-agent should assess whether the evidence is sufficient given what the spec required — tier 1–2 is strong support; tier 4 is a gap the human reviewer must close.

**Done when**: `/review` report presented to user.
