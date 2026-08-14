# Key Design Decisions

## MICTUNING P1B as PDM

MICTUNING is a **PDM** (Power Distribution Module), not a switch panel. Each channel provides fused power directly to the load. External relays only for starter (galvanic isolation).

## Ground: Star Topology

Each local ground bus (engine, cabin, trunk) connects independently to a common point. Daisy chain is **forbidden**.

## No Soldering

Crimp with adhesive heat-shrink only. Tug-test every connection.

## BATT ECU

BATT ECU gets constant +12V directly from the main bus (separate 10A fuse), **not through MICTUNING**.

## CH6 Stop Lights

CH6 (stop lights) activates automatically with IGN (CH1). Brake pedal switch breaks/makes the circuit from CH6 to lamps.

## Wire Gauge Reference

| Gauge (mm2) | Use |
|---|---|
| 50 | Battery cables, starter power |
| 6-10 | Alternator charge cable |
| 2.5 | High-current loads (heater, fan, fuel pump, main grounds) |
| 1.5 | Medium loads (headlights) |
| 0.75 | Low-current (parking, turn signals, ECU signals, OBD) |
