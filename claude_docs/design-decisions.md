# Key Design Decisions

## MICTUNING P1B as PDM

MICTUNING is a **PDM** (Power Distribution Module), not a switch panel. Each channel provides fused power directly to the load. External relays only for starter (galvanic isolation).

## Ground: Star Topology + Local Ground at Connector

Each local ground point on the body (engine bay, cabin, trunk) connects independently to a common point. Daisy chain is **forbidden**.

**Local Ground concept:** harnesses carry only +12V wires from MICTUNING. Each inline connector (X100, X200, X300, X400) has dedicated GND pins — one per consumer. The mating side of each connector connects these GND pins to the nearest body ground point with short wires (200–400 mm). No ground wires are routed through the harness back to the central panel.

**X400 (ECU harness):** power is taken from the same location as MICTUNING PDM — they are physically close. +12V for IGN (CH1) and BATT come from the PDM area, not from the central panel.

## No Soldering

Crimp with adhesive heat-shrink only. Tug-test every connection.

## BATT ECU

BATT ECU gets constant +12V directly from the main bus (separate 10A fuse), **not through MICTUNING**.

## Stop Lights — Direct from Main Bus

Stop pedal switch (C4, 2-pin) is powered directly from the main bus (+12V constant), not from MICT CH6. When pressed, +12V passes through the switch to splice s-6, which feeds X200.5 (left stop) and X200.6 (right stop). Brake lights work regardless of ignition state (safety). CH6 is now a reserve channel (10A).

## Fuses as Resistors in harness.design

In the harness.design schematic, **fuses are represented as resistors**. This is a visual convention — the physical harness uses real fuses (MICTUNING channel fuses, standalone fuses like F_MAIN, BATT ECU 10A, OBD 5A). Resistors in the schematic correspond 1:1 to fuses in the BOM. The resistor value/description field carries the fuse amperage (e.g. "10A", "30A").

## Wire Gauge Reference

| Gauge (mm2) | Use |
|---|---|
| 50 | Battery cables, starter power |
| 6-10 | Alternator charge cable |
| 2.5 | High-current loads (heater, fan, fuel pump, main grounds) |
| 1.5 | Medium loads (headlights) |
| 0.75 | Low-current (parking, turn signals, ECU signals, OBD) |
