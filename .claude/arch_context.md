# harness.design Editing Guide — Compressed Reference

Source: `read_editing_guide` MCP topics. Re-read from MCP if uncertain or before complex edits.

## Operations (edit_document)

- **add** — append element; `index` for insert position (order matters for cavities/cores)
- **update** — shallow-merge fields; `null` removes optional field; id cannot change
- **replace** — swap whole element; id must match
- **remove** — delete + auto-cascade (wires, mates, bundles, frame refs pointing at it)
- **set** — change doc setting: `currency`, `lengthUnit`, `gaugeUnit`, `formboard`

Operations apply **in order**, **all-or-nothing**. An op may reference ids added by earlier ops.

Nested collections (with `parentId`):
| collection | parentId is | element |
|---|---|---|
| cavities | connector | cavity |
| cores | cable | cable core |
| wires | twistedWires | inner wire |
| layoutPoints | wire or bundle | routing point |
| configurations | connectorPart | part config |
| cores | cablePart | part core |

## IDs

- Unique across whole document (all components + all parts)
- Existing ids never change; to renumber: remove + add (cascade applies)
- New ids: any unique string; convention: short prefix scheme

## Connection Model

Every conductor has `source` and `target`, each `{ "id", "handle" }`:
- `id` — element being connected (Connector, Terminal, Splice, Diode, Resistor)
- `handle` — cavity id for connectors; `"Terminal"` for terminals; `"Splice"` for splices; `"Left"`/`"Right"` for diodes/resistors

Both endpoints must reference existing elements/handles or cleaner drops the conductor.

## Connector

Fields: `id`, `label`, `schematicPosition`/`layoutPosition`, `cavities[]`, optional `shell` (Cavity), `partId`, `configurationId`, `width`, `rotated`, `coveringIds`, `excludeFromBom`

**Cavity**: `id` (used as wire handle), optional `signal`, optional `contactPartId`, rarely `designation`

**Part invariants** (when `partId` set):
- `cavities.length` == `ConnectorPart.numberOfCavities`
- has `shell` **iff** `ConnectorPart.hasShell` is true

**Contacts**: set via part `configuration.contactPartId` (applies to all cavities). Override per-cavity only for cavities needing a *different* contact.

**Designations**: never set `cavity.designation` by hand when connector has a part. Only set manually when no part.

**Signals**: set once at origin cavity; propagates along net. `propagate: false` stops flow; `global: true` shares across same-named signals.

## Terminal

Fields: `id`, `type` (required: Ferrule/Ring/Spade/QuickConnectMale/QuickConnectFemale/Loose), `schematicPosition`/`layoutPosition`, optional `signal`, `partId`, `width`, `rotated`, `coveringIds`, `excludeFromBom`

Wire connects with handle `"Terminal"`. Part gauge range should match connected wires.

## Splice

Fields: `id`, `schematicPosition`/`layoutPosition`, optional `locationId` (usually omit), `partId`, `coveringIds`, `excludeFromBom`

- Must join **at least 2 wires** (handle = `"Splice"`)
- Leave `locationId` undefined unless splice must be pinned to a location

## Diode / Resistor

Fields: `id`, `schematicPosition`/`layoutPosition`, `rotated`, optional `locationId`, `partId`, `coveringIds`, `excludeFromBom`

- Diode: `anodeOnRight` (default: anode left, cathode right)
- Wire one side to `"Left"`, other to `"Right"` — sits *between* two wires
- Resistor: same minus `anodeOnRight`

## Wire

Fields: `id`, `source`, `target`, `color` (required), optional `stripeColor`/`identColors`/`identLabel`, `partId`, `coveringIds`, `layoutPoints`

## Cable

Multi-core sheathed assembly. **Schematic-only** node (`schematicPosition`).

Fields: `id`, `schematicPosition`, `rotated`, `partId`, `cores[]`, optional `shield`

- Core: `id`, `color`, optional `stripeColor`, `source`, `target`. **No partId** — gauge/colour from parent CablePart cores
- Shield: optional core, may connect at only one end, `color: "Shield"`

## Twisted Wires

2–8 wires twisted. **Schematic-only** node.

Fields: `id`, `schematicPosition`, `rotated`, `wires[]` (each like a wire: `id`, `color`, `source`, `target`, etc.)

## Mates

Records physical plug-together. No wires of its own; continuity flows across.

Fields: `id`, `sourceId`, `targetId`, `targetCavityId` (only for terminal→connector)

- Connector↔connector: just sourceId/targetId (opposite genders)
- Terminal→connector: set `targetCavityId`
- Terminal↔terminal: just sourceId/targetId

## Bundles

Physical routing path in **layout view**.

Fields: `id`, `sourceId`, `targetId` (required; Connector/Terminal/BranchPoint), optional `length` (`{ value, unit }`), `label`, `coveringIds`, `layoutPoints`, `radius`

**Branch points** (layout-only): `id`, `layoutPosition` (required), optional `bootPartId`, `coveringIds`

Convention: bundle connects nodes (not cavities). Split trunk via branch point.

## Coverings

Applied by reference via `coveringIds` on: wires, connectors, terminals, splices, diodes, resistors, bundles, branch points.

CoveringPart: `type` (HeatShrink/CorrugatedTubing/SpiralWrap/Tape/Tubing/BraidedSleeve/Other), diameter range, part number.

## Positioning

- Grid: 30-unit multiples. Position = top-left corner.
- Every component needs at least one position field
- **Schematic**: perimeter rectangle (endpoints on edges), inline components in center
- **Layout**: positions roughly match real physical locations
- Node sizes (schematic, non-rotated):
  - Connector: width `w` × height `30 + N×30` (+30 if shell)
  - Terminal: width `w` × `30`
  - Cable: `60` × `30 + N×30`
  - TwistedWires: `90` × `30 + N×30`
  - Splice/Diode/Resistor: `30 × 30`
- Layout view: all nodes `30 × 30`
- Leave 60–90px clear space between nodes

## Key Rules

1. **Send only what changes.** Never re-send elements you're not editing.
2. **Parts: reuse library → research → follow doc convention.** Add parts to current document by default, library only on explicit request.
3. **Cascade is automatic** on remove — don't hand-remove wires/mates/bundles pointing at deleted element.
4. **Validate before risky edits** — `dryRun: true` on edit_document.
5. **Document = harness only.** External devices (ECU, sensors) → harness-side connector(s) in a frame named for the device.
6. **Match existing conventions** in the document rather than imposing your own.
7. **Formboard**: never toggle by adding/removing `formboard` field.
