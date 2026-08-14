# Architecture

## Modular Harness Structure

The harness is modular with three physical sub-harnesses connected through inline connectors to a central panel:

- **Central Panel** (`architecture/01-central-panel.md`) — MICTUNING P1B (12 channels), main + bus, kill switch, starter relay, ground star topology
- **Front Harness** (`architecture/05-front-harness.md`) — X100 (11-pin), headlights, cooling fan, starter relay control, alternator, front parking/turn signals
- **Rear Harness** (`architecture/02-rear-harness.md`) — X200 (8-pin), fuel pump, tail/stop/turn lights
- **Cabin Harness** (`architecture/03-cabin-harness.md`) — X300 (6-pin), heater, wipers, brake switch, OBD-II
- **ECU Wiring** (`architecture/04-ecu-wiring.md`) — IGN (CH1), BATT (direct), STA signal; engine-side wiring is stock Toyota

## Cross-Module Connections

CH10/CH11/CH12 split at the panel — one branch to front harness, one to rear.

## Inline Connectors

| Connector | Pins | Modules | Purpose |
|-----------|------|---------|---------|
| X100 | 11 | Panel ↔ Front | Headlights, fan, starter, alternator, parking/turn |
| X200 | 8 | Panel ↔ Rear | Fuel pump, tail/stop/turn lights |
| X300 | 6 | Panel ↔ Cabin | Heater, wipers, brake switch, OBD-II |
