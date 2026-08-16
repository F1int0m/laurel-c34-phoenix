# Editing Guide — Topic Index

Compressed summaries of each `read_editing_guide` topic. If this summary is
insufficient, call `read_editing_guide('<topic>')` for the full reference.

## editing-basics

**Write model:** `edit_document` applies targeted operations in order, all-or-nothing. Send only what changes — never re-send elements you aren't editing. `set_document_json` is for wholesale replacement only.

**Operations:**
- `add` — append/insert a complete element. `index` for position (order matters for cavities/cores).
- `update` — shallow-merge fields you list; `null` removes optional field. Id cannot be changed.
- `replace` — swap whole element (id must match).
- `remove` — delete element; cascade is automatic (see below).
- `set` — change document setting: `currency`, `lengthUnit`, `gaugeUnit`, `formboard`.

**Nested collections** (use `parentId`): `cavities` (connector), `cores` (cable), `wires` (twistedWires), `layoutPoints` (wire/bundle), `configurations` (connectorPart), `cores` (cablePart).

**Orienting before edit:** `get_harness_summary` for shape, `get_component_ids` to resolve display IDs to internal IDs, `get_document_json(fields=[...])` for the parts you're changing.

**Cascade rules:** Removing a component auto-cleans: wires whose source/target pointed at it, mates and bundles referencing it, frame elements entries, and partId/contactPartId/coveringIds references. The response reports everything removed/modified — read it. Do NOT hand-remove cascaded items yourself.

**Parts reuse:** 1) Reuse from library (`get_part_library`). 2) Research named parts. 3) Follow document convention (often no part). Add parts to current document by default; only `edit_part_library` when user asks.

**IDs:** Unique across the whole document. Existing IDs never change. New IDs are any unique string.

**Positions:** Every component needs at least one of `schematicPosition`/`layoutPosition`. See **positioning** for grid conventions.

**Validate:** `dryRun: true` on `edit_document` to test without writing. `validate_harness` health-checks stored document.

## connectors

**Fields:** `id`, `label`, `schematicPosition`/`layoutPosition`, `cavities[]`, `shell` (optional Cavity), `partId`, `configurationId`, `width`, `rotated`, `coveringIds`, `excludeFromBom`.

**Cavity:** `id` (used as wire handle), optional `signal`, optional `contactPartId`, rarely `designation`. Shell is also a Cavity — its `id` is the handle for shield wires.

**Part invariants:** When `partId` is set: `cavities.length` must equal `ConnectorPart.numberOfCavities`; connector has `shell` iff `part.hasShell` is true. `validate_harness` flags mismatches.

**Configurations:** A ConnectorPart may define `configurations` — named combos of accessories + default contact. First config is default. Set `connector.configurationId` only for non-default. `configuration.contactPartId` applies to every cavity; only override per-cavity `contactPartId` for cavities needing a different contact.

**Designations:** Never set `cavity.designation` when connector has a part — labels come from the part's `designationStrategy`. Only set manually when no part.

**Signals:** Propagate along the net. Set `signal` once at origin; it flows to connected cavities. Use `propagate: false` to stop, `global: true` to share across same-named signals.

**Resolving pins:** User says "pin A3 of C14" — call `get_component_ids` to map display label to internal cavity id. Don't guess.

```json
{ "id": "J1", "label": "J1", "schematicPosition": { "x": 90, "y": 210 },
  "partId": "cp_2f", "cavities": [ { "id": "j1c1", "signal": "PWR" }, { "id": "j1c2" } ] }
```

## wiring

**Wire** (in `wires`): `id`, `source`, `target` (each `{id, handle}`), `color` (required PartColor), `stripeColor`/`identColors`/`identLabel` (optional), `partId` (WirePart), `coveringIds`, `layoutPoints`.

**Connection model:** `handle` = cavity id (or shell id), `"Terminal"`, `"Splice"`, `"Left"`/`"Right"` (diode/resistor). Both endpoints must exist or cleaner drops the conductor.

**Cable** (multi-core, schematic-only): `id`, `schematicPosition`, `partId`, `cores[]` (each: `id`, `color`, `stripeColor`, `source`, `target`), optional `shield` core. Cores have no `partId` — they inherit gauge/colour from parent CablePart matched by position.

**Twisted wires** (schematic-only): `id`, `schematicPosition`, `wires[]` (2-8 inner conductors, each like a wire).

```json
{ "id": "w1", "source": { "id": "J1", "handle": "j1c1" },
  "target": { "id": "J2", "handle": "j2c1" }, "color": "Red", "stripeColor": "White", "partId": "wp_18rd" }
```

## splices

**Splice** (in `splices`): `id`, `schematicPosition`/`layoutPosition`, `locationId` (optional — usually omit), `partId` (SplicePart, has gauge range), `coveringIds`, `excludeFromBom`.

**Conventions:** Must join at least 2 wires. `validate_harness` warns if fewer. Leave `locationId` undefined unless splice must be pinned to a specific location.

**Wire connection:** All wires point at splice with handle `"Splice"`.

```json
{ "id": "s1", "schematicPosition": { "x": 390, "y": 210 } }
// wire: { "id": "w1", "source": { "id": "J1", "handle": "j1c1" }, "target": { "id": "s1", "handle": "Splice" }, "color": "Red" }
```

## terminals

**Terminal** (in `terminals`): `id`, `type` (required: Ferrule, Ring, Spade, QuickConnectMale, QuickConnectFemale, Loose), `schematicPosition`/`layoutPosition`, `signal`, `partId` (TerminalPart), `width`, `rotated`, `coveringIds`, `excludeFromBom`.

**Conventions:** Multiple wires can connect, all with handle `"Terminal"`. TerminalPart gauge range (`minGauge`/`maxGauge`) should fit connected wires. Keep `type` and `TerminalPart.type` consistent.

```json
{ "id": "t1", "type": "Ring", "schematicPosition": { "x": 210, "y": 210 }, "signal": "GND", "partId": "tp_ring" }
```

## bundles

**Bundle** (in `bundles`): `id`, `sourceId`, `targetId` (required; Connector, Terminal, or BranchPoint), `length` (`{value, unit}`), `label`, `labelColor`, `coveringIds`, `layoutPoints`, `radius`.

**BranchPoint** (in `branchPoints`): `id`, `layoutPosition` (required), `bootPartId` (optional), `coveringIds`.

**Conventions:** Bundle connects two nodes, not cavities. Split a trunk with a branch point: one bundle in, multiple out. `length` is physical segment for manufacturing/BOM. In Formboard mode, drawn length should match set length; use app's "Match" action.

```json
{ "id": "bp1", "layoutPosition": { "x": 690, "y": 390 } }
{ "id": "bnA", "sourceId": "J1", "targetId": "bp1", "length": { "value": 250, "unit": "mm" }, "label": "Main Trunk" }
{ "id": "bnB", "sourceId": "bp1", "targetId": "J2", "length": { "value": 120, "unit": "mm" } }
```

## mates

**Mate** (in `mates`): `id`, `sourceId`, `targetId`, `targetCavityId` (required only when terminal mates into a connector).

**Three types:**
- Connector ↔ connector: just `sourceId`/`targetId`. Normally opposite genders.
- Terminal → connector: add `targetCavityId` for the cavity the terminal enters.
- Terminal ↔ terminal: just `sourceId`/`targetId`.

**Conventions:** Mates stay inside the harness. Use a frame for external device relationships. Signals propagate through mates.

```json
{ "id": "m1", "sourceId": "J1", "targetId": "J2" }
{ "id": "m2", "sourceId": "t1", "targetId": "J3", "targetCavityId": "j3c2" }
```

## coverings

Coverings are applied by reference, not added as components. Covered element carries `coveringIds` — array of CoveringPart ids. Coverable elements: wires, connectors, terminals, splices, diodes, resistors, bundles, branch points.

**CoveringPart** schema: `type` (HeatShrink, CorrugatedTubing, SpiralWrap, Tape, Tubing, BraidedSleeve, Other), `minDiameter`/`maxDiameter`, optional fixed length, pricing.

**Conventions:** Choose covering whose diameter range fits what it wraps. On a bundle = whole run; on a wire = just that wire. Reuse existing CoveringPart before creating new.

```json
// bundle with covering:
{ "id": "b1", "sourceId": "J1", "targetId": "bp1", "coveringIds": ["cov_bs"] }
// covering part:
{ "id": "cov_bs", "type": "BraidedSleeve", "partNumber": "PT2-...", "minDiameter": { "value": 6, "unit": "mm" }, "maxDiameter": { "value": 12, "unit": "mm" } }
```
