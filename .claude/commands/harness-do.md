---
argument-hint: [harness directory with HARN-PLAN.md]
description: Execute a harness build plan via the harness-builder agent. Input is harnesses/HARN-XXXX/ with HARN-PLAN.md.
---

# Harness Do — Execute Build Plan

You are orchestrating the execution of a harness build plan. Your job is to hand the plan to the **harness-builder** agent and oversee the build through completion.

## Input Interpretation

- If `$ARGUMENTS` is a path to `harnesses/HARN-XXXX-<name>/` — read the `HARN-PLAN.md` file inside it.
- If `$ARGUMENTS` is a direct path to a `HARN-PLAN.md` file — read it.
- If no arguments — look for the most recent `HARN-PLAN.md` in the `harnesses/` directory.

## Workflow

### Step 1: Read the Plan

Read the `HARN-PLAN.md` to understand:
1. **Target harness** — new or existing (documentId)
2. **Build order** — dependency graph of phases
3. **Scope per phase** — parts, connectors, terminals, splices, wires, bundles, mates

### Step 2: Check Current State

If the target harness exists in **harness.design**:
- `list_harnesses` → locate the harness
- `get_harness_summary` → determine what has already been built
- `get_component_ids` → determine which IDs are already in use

If the harness does not exist:
- `create_harness` → create a new harness document

### Step 3: Execute via harness-builder Agent

For each phase in the build plan:

1. **Prepare the scope** — extract the phase's operations (parts, connectors, wires, etc.) from the plan
2. **Launch the harness-builder agent** with:
   - The phase scope (what to build)
   - The current harness documentId
   - Any existing component IDs that must not be duplicated
3. **Verify** — after each phase, check that the expected components exist using `get_component_ids`

### Step 4: Cross-Module Connections

If the plan has cross-module connections between phases:
- Execute these after all individual phases are complete
- Pass the relevant connectors and circuits to the harness-builder

### Step 5: Final Verification

Run the full verification checklist from `HARN-PLAN.md`:
- All connectors have the correct cavity count
- All wires connect valid endpoints
- All splices join existing circuits
- No duplicate IDs
- Every bundle contains at least one wire
- Every mate references existing connectors

### Step 6: Report

Produce a build report:
- What was built (components count by type)
- What was skipped (already exists)
- Any issues encountered
- BOM summary via `get_bom`

## Rules

1. **Follow the plan.** Do not deviate from `HARN-PLAN.md`. If the plan is incomplete, stop and ask.
2. **One phase at a time.** Phases have dependencies — do not parallelize independent phases unless the plan explicitly allows it.
3. **Idempotent.** Before building, check what already exists. Skip, don't duplicate.
4. **Agent delegation.** Use the **harness-builder** agent for actual construction. You are the orchestrator, not the builder.
5. **Announce edits.** Before each `edit_document` call (via the agent), `announce_editing(documentId)` must be called.

## Error Handling

- **Plan references non-existent component** → stop, report, ask for clarification
- **Build fails (dryRun or execute)** → read the error, fix if obvious, otherwise stop and report
- **Harness not found** → create a new one and continue
- **MCP token expired** → inform the user
