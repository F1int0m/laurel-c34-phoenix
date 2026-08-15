# Architecture

## Modular Harness Structure

The harness is modular with four physical sub-harnesses connected through inline connectors to a central panel. **Each harness carries only +12V wires from MICTUNING; ground is provided locally at the connector mating side (see Local Ground concept in `architecture/00-context.md`).**

- **Central Panel** (`architecture/01-central-panel.md`) — MICTUNING P1B (12 channels), main + bus, kill switch, starter relay, ground star topology
- **Front Harness** (`architecture/05-front-harness.md`) — X100 (20-pin), headlights, cooling fan, starter relay control, alternator, front parking/turn signals
- **Rear Harness** (`architecture/02-rear-harness.md`) — X200 (16-pin), fuel pump, tail/stop/turn lights
- **Cabin Harness** (`architecture/03-cabin-harness.md`) — X300 (8-pin), heater, wipers, brake switch, OBD-II
- **ECU Wiring** (`architecture/04-ecu-wiring.md`) — X400 (5-pin), IGN (CH1), BATT (direct), STA signal; engine-side wiring is stock Toyota

## Cross-Module Connections

CH10/CH11/CH12 split at the panel — one branch to front harness, one to rear.

## Inline Connectors

| Connector | Pins | Modules | Purpose |
|-----------|------|---------|---------|
| X100 | 20 | Panel ↔ Front | Headlights, fan, starter, alternator, parking/turn + dedicated GND per consumer |
| X200 | 16 | Panel ↔ Rear | Fuel pump, tail/stop/turn lights + dedicated GND per consumer |
| X300 | 6 | Panel ↔ Cabin | Heater, wipers, OBD-II + dedicated GND per consumer |
| X400 | 5 | PDM ↔ ECU | IGN (CH1), BATT, STA + GND (IGN, BATT); power taken from PDM area (physically close) |

**Stop Pedal (C4):** standalone 2-pin connector in the cabin, powered directly from the main bus (constant +12V). Pin 1 (In) ← main bus; Pin 2 (Out) → splice s-6 → X200.5/X200.6 (rear stop lights). Not routed through X300. Brake lights work regardless of ignition state.
