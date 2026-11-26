# vibo-selfdeed — usage examples

Three typical cases. Each one is a brief the owner gives the agent.
The agent follows SKILL.md (START → SCAN → PROPOSE → FIX → ITERATE → LEARN → REPORT).

## Example 1 — code: "Find and fix the bugs in src/"

**Task:** tests fail in `src/` and there is duplicated code.

```
./run_mission.sh init --task "fix failing tests in src/" --target 90
python3 lib_vibo.py find "src tests architecture"      # START: context
./run_mission.sh checkpoint START ok "project is a Flask API, 3 tests failing"
# SCAN: pytest, duplication grep → 3 bugs
./run_mission.sh checkpoint SCAN ok "3 bugs: NPE in routes, timeout, duplicate helper"
# PROPOSE: diffs → confirmation (or --auto)
# FIX: safety.backup("src/routes.py") → edits → pytest
# ITERATE: path A did not fix the third bug → switch to B
# LEARN: vibo add lesson "timeout bug is fixed with asyncio.timeout, not sleep"
./run_mission.sh checkpoint LEARN ok "lesson saved"
```

## Example 2 — documents: "Proofread docs/ and bring it to one style"

**Task:** 12 files in `docs/`, inconsistent style (headings, terms).

```
./run_mission.sh init --task "single style for docs/ (headings + terms)" --target 100
python3 lib_vibo.py find "docs style terms"
# SCAN: grep headings (# vs ##), terms (ViBo vs VIBO) → 8 mismatches
# PROPOSE → FIX (back up every file)
# LEARN: vibo add lesson "standard: H2 for sections, one spelling of 'ViBo'"
```

## Example 3 — configs: "Check that configs agree with each other"

**Task:** `config/` — prod, staging and dev must share the same keys.

```
./run_mission.sh init --task "compare config/ prod vs dev" --target 100
# SCAN: compare keys (python yaml/json) → 2 missing keys
# PROPOSE → FIX (add keys, back up)
# LEARN: vibo add lesson "key X is required in every environment"
```

## Advice for the agent

- **SCAN is done by the agent itself** (lint, tests, grep, reading files). The skill provides structure and memory, not magic.
- **Memory is leverage:** run `vibo find` before every round — do not step on the same rake twice.
- **Paths A/B/C:** if an approach shows no progress for two rounds, switch; do not grind.
- **Safety:** back up before editing, three attempts, ten minutes, never touch L3.
- **Honesty:** do not invent "magic" — report only what actually happened.
