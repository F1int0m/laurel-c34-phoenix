---
name: wiring-context
description: Project context for Nissan Laurel C34 + 1UZ VVT-i wiring — components, decisions, parameters
metadata:
  type: project
---

# Wiring Project Context — Nissan Laurel C34 + 1UZ VVT-i

## Vehicle and Swap

- **Donor:** Nissan Laurel C34
- **Engine:** Toyota 1UZ VVT-i
- **Transmission:** Manual
- **ECU → engine wiring:** already complete (stock Toyota ECU); only needs +12V IGN supply
- **A/C:** removed

## Battery and Power Topology

- **Battery:** trunk-mounted
- **Battery → main bus+ cable:** 50 mm²
- **Main bus+:** cabin-mounted
- **Kill switch:** on positive line (between battery and main bus+)
- **Alternator:** stock 1UZ, 80–100A; dedicated 80A cable from main bus+ to alternator
- **Starter:** 50 mm² cable from main bus+ to starter (separate from MICTUNING)
- **Main fuse:** on power line from battery

## MICTUNING P1B — PDM Module

### Specifications

- **Model:** 12 gang (80A total current, 960W at 12V)
- **Voltage:** 12V DC
- **Protection:** IP65
- **Power:** direct from battery — all channels operate independently of ignition
- **Ground:** through housing (metal body mount)
- **4 control modes:**
  - Red — Toggle (on/off, constant power)
  - Green — Momentary (held)
  - Blue — Flash (intermittent, for turn signals)
  - Cyan — Pulsed Strobe (for hazard lights)

### Channel and Fuse Allocation — Approved

| Channel | Fuse | Function | MICT Mode |
|---------|------|----------|-----------|
| CH1 | 30A | IGN bus (ECU +12V) | Toggle |
| CH2 | 30A | Cooling fan | Toggle |
| CH3 | 20A | Fuel pump | Toggle |
| CH4 | 15A | Low beam (halogen) | Toggle |
| CH5 | 10A | Starter relay (low-current control) | Momentary |
| CH6 | 10A | Reserve | — |
| CH7 | 30A | Heater | Toggle |
| CH8 | 30A | Reserve (high-current) | — |
| CH9 | 20A | Wipers | Toggle |
| CH10 | 15A | Parking lights (front + rear) | Toggle |
| CH11 | 10A | Left turn signal | Flash |
| CH12 | 10A | Right turn signal | Flash |

**Stop lights:** stop pedal switch (C4, 2-pin) is powered directly from main bus+ (constant +12V, not through MICT). When the pedal is pressed, +12V passes through the switch → splice s-6 → X200.5/X200.6 (rear stop lights). Stop lights work regardless of ignition state. CH6 is now a free reserve channel.

**Starter relay (CH5):** Momentary mode — power is supplied only while the button is held. CH5 provides +12V to the coil of the external starter relay, which switches high-current + from main bus to starter.

**Turn signals (CH11/CH12):** MICTUNING Flash mode generates the intermittent signal. Each channel drives front + rear turn signals on one side.

### Key Principle

MICTUNING is a **PDM (Power Distribution Module)**, not just a switch panel. Each channel:
- Has its own fuse
- Can deliver power directly to the load (up to the channel fuse rating)
- Does not require external relays for loads up to 30A (channel fuse protects the circuit)

External relays are only needed when:
- Load exceeds the channel fuse rating
- Galvanic isolation is required (e.g., starter — low-current relay control)

### Fuses as Resistors in harness.design

In the harness.design schematic, **fuses are represented as resistors**. This is a visual convention — the physical harness uses real fuses (MICTUNING channel fuses, standalone fuses F_MAIN, BATT ECU 10A, OBD 5A, etc.). Each resistor on the schematic corresponds 1:1 to a fuse. The resistor value/description field carries the fuse amperage (e.g. "10A", "30A").

## Loads — What We Design

### Circuits to design

- Headlights (halogen, ~10A per pair)
- Cooling fan (1 unit)
- Fuel pump (upgraded aftermarket, ~10–15A)
- Starter relay (low-current control from MICT CH5)
- Stop light (pedal switch, powered from main bus+, not through MICT)
- Turn signals (via MICT Flash mode)
- Heater (stock Laurel motor, ~15–20A)
- Wipers (stock Laurel motor, ~5–10A)
- ECU IGN signal (+12V after ignition)
- OBD port (for digital dash)

### Circuits NOT needed

- Reverse light — no
- Horn — no
- Interior light — no
- A/C — removed
- Neutral safety switch — no (manual trans, driver controls)

### Out-of-scope circuits (already complete)

- ECU → engine (sensors, injectors, coils, MAF, O2, etc.) — stock Toyota wiring
- ECU only needs a +12V IGN wire

## Control

- **All light and load control — through MICTUNING** (no separate column stalk switches)
- **Instrument cluster:** digital dash via OBD

## Harness Architecture (from existing files)

Modular design with sub-harnesses:
- [[01-central-panel]] — central panel (MICTUNING, + and GND buses)
- [[02-rear-harness]] — rear (tail lights, fuel pump)
- [[03-cabin-harness]] — cabin (heater, wipers, OBD)
- [[04-ecu-wiring]] — ECU power supply
- [[05-front-harness]] — front (headlights, fan, alternator, starter)

## Local Ground at Connector Concept

**Principle:** from MICTUNING to each harness, **only +12V wires** run. Ground wires are not routed through the harness back to the central panel.

Each harness ends with an inline connector (X100, X200, X300, X400). Besides signal (+) pins, each connector has **dedicated GND pins** — one per consumer or consumer group. On the **mating side** of each connector, these GND pins connect to the **nearest body ground point** with short wires.

**X400 (ECU harness)** — power is taken from the same location as MICTUNING PDM, since PDM and ECU are physically close. This is intentional: instead of routing wires from the central panel, +12V for IGN (CH1) and BATT are taken locally from the PDM.

### Why

- Eliminates long ground runs through the entire harness (up to 1.5–2 m each)
- Ground is local — less voltage drop, less copper weight
- Each harness becomes "positive-only" — simpler assembly and diagnostics
- Ground buses (engine bay, cabin, trunk) remain, but are now fed from connector mating sides, not from the central panel

### Rules

1. **Each consumer** in the harness gets its own GND pin in the connector (or one GND for a group of low-current consumers)
2. **Connector mating side** — each GND pin connects to the body with a short wire (200–400 mm) to the nearest ground point
3. **GND pin gauge** = gauge of the positive wire for the same consumer
4. **Common ground bus** in each zone (engine bay, trunk, cabin) remains — this is the body point where GND wires from connector mating sides converge
5. **MICTUNING** still grounds through its housing (unchanged)
6. **High-current circuits** (starter 50 mm², alternator) — unchanged; engine/alternator ground to body directly

### Connector Changes

| Connector | Before | After |
|-----------|--------|-------|
| X100 (front harness) | 1 shared GND pin | GND pin per consumer: headlights L/R, fan, parking L/R, turn signals L/R, starter relay |
| X200 (rear harness) | 1 shared GND pin | GND pin per consumer: parking, turn signals L/R, stop lights, fuel pump |
| X300 (cabin harness) | 1 shared GND pin | GND pin per consumer: heater, wipers, OBD |
| X400 (ECU harness) | — | GND pin per consumer: GND IGN, GND BATT |
