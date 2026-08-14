# MCP Integration

## harness.design Server

The project uses the **harness-design** MCP server to build the harness visually in [harness.design](https://app.harness.design).

- **Configuration:** `.mcp.json` (gitignored; see `.mcp.json.example` for template)
- **Active harness:** "Laurel harness" (documentId: `K1vY`)

## Custom Commands

- `/harness-feat` — guided requirements gathering, outputs `harnesses/HARN-XXXX/README.md`
- `/harness-plan` — builds construction plan from brief, outputs `harnesses/HARN-XXXX/HARN-PLAN.md`

## Custom Agent: harness-builder

Executes build plan in harness.design via MCP using PLAN→DRY-RUN→EXECUTE→VERIFY methodology. **Never designs**; only implements a given spec. Build order: Parts → Connectors+Cavities → Terminals → Splices → Wires → Bundles → Mates.
