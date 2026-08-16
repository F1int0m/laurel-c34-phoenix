# Front Harness — X100

## Purpose

The front harness connects the central panel (MICTUNING PDM, main bus+) to engine bay consumers: low beam headlights, cooling fan, starter (via relay), alternator, front parking lights, and front turn signals. ECU power circuits (IGN, BATT, STA) pass through the front harness physically but are described in [[04-ecu-wiring]]. The harness connects to the panel via connector **X100** (20-pin). Power cables (starter 50 mm², alternator 6-10 mm²) connect to bus+ and the starter relay via ring terminals, bypassing connector X100.

**Local ground concept:** from MICTUNING to the harness, ONLY +12V wires run. No ground wires are routed through the harness back to the panel. At the end of the harness, connector X100 has dedicated GND pins in addition to + pins — one per consumer. On the mating side of the connector, these GND pins connect to the nearest body ground point with short wires (200-400 mm).

## What is NOT in the Front Harness

- **ECU power (IGN, BATT, STA)** — described in [[04-ecu-wiring]], though wires pass through the front harness
- **ECU sensors, injectors, coils** — stock Toyota wiring (already complete)
- **A/C** — removed
- **Horn** — not needed
- **Stop lights** — Stop Pedal (direct power from main bus) → splice s-6 → X200
- **Rear parking lights / rear turn signals** — via rear harness (X200)

## Routing

From the central panel, the harness passes through the firewall into the engine bay. Branches run to the headlights (left/right), radiator fan, starter relay, and alternator.

```
Central Panel (MICTUNING PDM, Bus+)
    │
    ├── X100 (20-pin) ──→ Left headlight (low beam, CH4) + GND
    │                 ──→ Right headlight (low beam, CH4) + GND
    │                 ──→ Left front parking light (CH10) + GND
    │                 ──→ Right front parking light (CH10) + GND
    │                 ──→ Left front turn signal (CH11) + GND
    │                 ──→ Right front turn signal (CH12) + GND
    │                 ──→ Cooling fan (CH2, direct PDM power) + GND
    │                 ──→ Starter relay (CH5, low-current control) + GND
    │
    │  X100 GND pins → mating side → short wires → body ground points
    │
    ├── Ring terminal ──→ 50 mm² ──→ Starter relay ──→ 50 mm² ──→ Starter
    │
    └── Ring terminal ──→ 6-10 mm² ──→ Alternator (B/S, charge cable)
```

## Connector X100 — Pinout

Connector X100 is 20-pin. It contains 8 +12V circuits and 8 dedicated GND pins (one per consumer). On the mating side, each GND pin connects via a short wire (200-400 mm) to the nearest body ground point.

### +12V Circuits

| Pin | Signal | Destination | Gauge (mm²) | Notes |
|-----|--------|-------------|--------------|-------|
| 1 | +12V (MICT CH2) | Fan (+) | 2.5 | Direct PDM power, 30A |
| 2 | +12V (MICT CH4) | Left headlight (low beam +) | 1.5 | |
| 3 | +12V (MICT CH4) | Right headlight (low beam +) | 1.5 | |
| 4 | +12V (MICT CH5) | Starter relay coil (+) | 0.75 | Low-current control |
| 5 | +12V (MICT CH10) | Left front parking (+) | 0.75 | |
| 6 | +12V (MICT CH10) | Right front parking (+) | 0.75 | |
| 7 | +12V (MICT CH11) | Left front turn signal (+) | 0.75 | Flash |
| 8 | +12V (MICT CH12) | Right front turn signal (+) | 0.75 | Flash |

### Dedicated GND Pins

| Pin | Signal | Destination (mating side) | Gauge (mm²) | Notes |
|-----|--------|---------------------------|--------------|-------|
| 9 | GND headlight left | Body ground point (200-400 mm) | 1.5 | Low beam |
| 10 | GND headlight right | Body ground point (200-400 mm) | 1.5 | Low beam |
| 11 | GND fan | Body ground point (200-400 mm) | 2.5 | |
| 12 | GND parking left | Body ground point (200-400 mm) | 0.75 | |
| 13 | GND parking right | Body ground point (200-400 mm) | 0.75 | |
| 14 | GND turn signal left | Body ground point (200-400 mm) | 0.75 | |
| 15 | GND turn signal right | Body ground point (200-400 mm) | 0.75 | |
| 16 | GND starter relay coil | Body ground point (200-400 mm) | 0.75 | Relay coil ground |

### Reserve

| Pin | Signal | Notes |
|-----|--------|-------|
| 17 | NC | Reserve |
| 18 | NC | Reserve |
| 19 | NC | Reserve |
| 20 | NC | Reserve |

> **Power cables** (50 mm² to starter, 6-10 mm² to alternator) connect to bus+ and starter relay via ring terminals — they do not pass through connector X100. Starter and alternator grounds go to the body directly, not through the connector.

## Front Harness Circuit Table

### Power Circuits from MICTUNING PDM

| ID | From | To | Length (mm) | Gauge (mm²) | Color | Fuse | MICT Ch | Notes |
|----|------|----|-------------|--------------|-------|------|---------|-------|
| P03 | MICT CH2 (Fan) | Fan (+) | ≈ 800 | 2.5 | Blue | 30A | CH2 | Direct PDM power |
| P04 | MICT CH4 (Light) | Left headlight (low beam +) | ≈ 800 | 1.5 | Red | 15A | CH4 | |
| P05 | MICT CH4 (Light) | Right headlight (low beam +) | ≈ 800 | 1.5 | Black | 15A | CH4 | |
| P06 | MICT CH5 (Start) | Starter relay coil (+) | ≈ 600 | 0.75 | Green | 10A | CH5 | Low-current control (coil ~0.15A) |
| P07 | MICT CH10 (Park) | Left front parking (+) | ≈ 800 | 0.75 | White | 15A | CH10 | |
| P08 | MICT CH10 (Park) | Right front parking (+) | ≈ 800 | 0.75 | Light Blue | 15A | CH10 | |
| P09 | MICT CH11 (L Turn) | Left front turn signal (+) | ≈ 800 | 0.75 | Yellow | 10A | CH11 | Flash |
| P10 | MICT CH12 (R Turn) | Right front turn signal (+) | ≈ 800 | 0.75 | Green | 10A | CH12 | Flash |

### High-Current Circuits (direct from bus+, bypassing MICTUNING)

| ID | From | To | Length (mm) | Gauge (mm²) | Color | Fuse | Notes |
|----|------|----|-------------|--------------|-------|------|-------|
| P20 | Bus+ (cabin) | Starter relay (power input) | ≈ 600 | 50 | Red | — | Starter cable, ring terminal |
| P21 | Starter relay (power output) | Starter | ≈ 1500 | 50 | Red | — | Starter cable, ring terminal |
| P22 | Bus+ (cabin) | Alternator (B/S) | ≈ 1500 | 6-10 | Red | 80A | Charge cable, ring terminal |

> **Principle:** Starter (50 mm²) and alternator (6-10 mm²) power cables connect to main bus+ directly — bypassing MICTUNING. These are separate circuits that do not pass through the PDM.

### Front Harness Ground Circuits (wires on mating side of X100)

Ground wires do not run through the harness from consumers back to the panel. Instead, each consumer gets a dedicated GND pin in connector X100, and on the mating side a short wire runs from that pin to the nearest body ground point.

| ID | From (X100 GND pin) | To | Length (mm) | Gauge (mm²) | Color | Notes |
|----|---------------------|----|-------------|--------------|-------|-------|
| G01 | Pin 9 (GND headlight left) | Body ground point | ≈ 200-400 | 1.5 | Black | Low beam |
| G02 | Pin 10 (GND headlight right) | Body ground point | ≈ 200-400 | 1.5 | Black | Low beam |
| G03 | Pin 11 (GND fan) | Body ground point | ≈ 200-400 | 2.5 | Black | |
| G04 | Pin 12 (GND parking left) | Body ground point | ≈ 200-400 | 0.75 | Black | |
| G05 | Pin 13 (GND parking right) | Body ground point | ≈ 200-400 | 0.75 | Black | |
| G06 | Pin 14 (GND turn signal left) | Body ground point | ≈ 200-400 | 0.75 | Black | |
| G07 | Pin 15 (GND turn signal right) | Body ground point | ≈ 200-400 | 0.75 | Black | |
| G08 | Pin 16 (GND starter relay coil) | Body ground point | ≈ 200-400 | 0.75 | Black | Relay coil ground |

> **Principle:** Within the harness itself (from panel to connector X100), there are no ground wires — only +12V. All grounds connect on the mating side of X100 with short wires to body ground points.

## Connection Details

### Low Beam (CH4)

Two halogen headlights connected in parallel to a single MICT CH4 channel (15A, Toggle). +12V power is supplied **directly from the PDM** — no external relays, since MICTUNING is a PDM that delivers power directly to the load. Each headlight has its own wire from CH4 (P04, P05), gauge 1.5 mm² (halogen bulb ~5A). Each headlight grounds through a dedicated X100 GND pin (Pin 9, Pin 10), connecting on the mating side to a body ground point via short wire (G01, G02).

**Total current:** ~10A (two bulbs at ~5A each). CH4 fuse — 15A, adequate margin.

### Cooling Fan (CH2)

One cooling fan powered **directly from MICT CH2 (30A, Toggle)** — no external relays, since the PDM delivers power directly. Wire gauge 2.5 mm². Fan grounds through dedicated X100 GND pin (Pin 11), connecting on the mating side to a body ground point via short wire (G03).

**Fan current:** ~15-20A. CH2 fuse — 30A, adequate margin.

> **Note:** Fan control is manual (MICTUNING Toggle). For automatic temperature-based operation, an additional thermo-switch and control circuit would be needed — this is an open question.

### Starter Relay (CH5)

MICT CH5 (10A, Momentary) supplies +12V to the coil of the external starter relay — this is a **low-current control circuit** (relay coil ~0.15A, gauge 0.75 mm²). The starter relay switches the high-current 50 mm² from main bus+ to the starter.

**Wiring diagram:**

```
Main bus+ ──── 50 mm² ──── Starter relay (power contacts) ──── 50 mm² ──── Starter
                                      │
                                 Relay coil
                                  │        │
                           CH5 (+12V)    GND (G08)
                          (Momentary)
```

1. Starter button held → CH5 supplies +12V (Momentary) to relay coil
2. Relay closes power contacts → 50 mm² from bus+ to starter
3. Starter cranks
4. Button released → CH5 turns off → relay opens → starter stops

STA signal to ECU — from CH5 or from the relay power output — so the ECU knows the starter is engaged (see [[04-ecu-wiring]] and open question #2).

### Alternator (Charge Cable)

Power cable 6-10 mm² from main bus+ to alternator B/S terminal — this is the **charge cable** (80A). Alternator is stock 1UZ, 80-100A. Cable connects via ring terminal, bypassing MICTUNING and connector X100.

L signal (charge warning lamp) from the alternator — in stock Toyota wiring. No separate circuit needed in this project.

### Engine ECU (CH1 + BATT + STA)

ECU power circuits (IGN, BATT, STA) physically pass through the front harness but are described in [[04-ecu-wiring]].

### Front Parking Lights (CH10)

Two front parking lights powered in parallel from MICT CH10 (15A, Toggle). This is the same circuit that feeds the rear parking lights via the rear harness (X200). Front parking lights go through the front harness (P07, P08), rear ones through the rear harness. Both pairs are powered from the same CH10 channel.

**Current per parking light:** <1A (5W bulb). Gauge 0.75 mm².

### Front Turn Signals (CH11/CH12)

Left front turn signal powered from CH11 (Flash), right front from CH12 (Flash). Rear turn signals on the same side go through the rear harness. Each MICT channel (CH11/CH12) powers front + rear turn signal on one side.

**Current per turn signal:** ~1.7A (21W bulb). Gauge 0.75 mm².

## Engine Bay Ground Points

Engine bay ground points are body points where short GND wires (200-400 mm) from the mating side of connector X100 arrive. Each body ground point connects **via a separate wire directly to the common point** (battery- / body) — star topology, no daisy chain.

1. **Ground point — left side (closest to headlight/parking/turn signal)** — connects:
   - GND headlight left (G01, Pin 9)
   - GND parking left (G04, Pin 12)
   - GND turn signal left (G06, Pin 14)

2. **Ground point — right side (closest to headlight/parking/turn signal)** — connects:
   - GND headlight right (G02, Pin 10)
   - GND parking right (G05, Pin 13)
   - GND turn signal right (G07, Pin 15)

3. **Ground point — fan** — connects:
   - GND fan (G03, Pin 11)

4. **Ground point — starter relay** — connects:
   - GND starter relay coil (G08, Pin 16)

5. **Engine ground** — heavy wire from engine block to common ground point (star). Alternator and starter grounds connect here directly, not through connector X100.

6. **Ground topology — star:** Each body ground point connects **via a separate wire directly to the common point** (battery- / body), not in series (daisy chain). This eliminates floating grounds.

## Wire Gauge Justification

| Consumer | Current (A) | Gauge (mm²) | Justification |
|----------|-------------|--------------|---------------|
| Low beam (each headlight) | ~5 | 1.5 | Halogen H4/H7 55W, standard gauge |
| Cooling fan | 15-20 | 2.5 | Single fan, direct PDM power (CH2, 30A) |
| Starter relay control (coil) | ~0.15 | 0.75 | Low-current circuit, relay coil current |
| Front parking lights (each) | <1 | 0.75 | 5W bulbs |
| Front turn signals (each) | ~1.7 | 0.75 | 21W bulbs |
| Starter power cable | 200-400 (cranking) | 50 | Cranking current, long run |
| Alternator charge cable | up to 80 | 6-10 | 1UZ alternator rated current |
| Fan GND (G03) | 15-20 | 2.5 | Matches power circuit |
| Headlight GND (G01, G02) | ~5 | 1.5 | Matches power circuit |
| Parking GND (G04, G05) | <1 | 0.75 | Matches power circuit |
| Turn signal GND (G06, G07) | ~1.7 | 0.75 | Matches power circuit |
| Starter relay coil GND (G08) | ~0.15 | 0.75 | Matches power circuit |

## Responsibility Boundary with Rear Harness

| Circuit | Front Harness (X100) | Rear Harness (X200) |
|---------|----------------------|---------------------|
| Low beam (CH4) | Both headlights | — |
| Parking lights (CH10) | Front parking | Rear parking |
| Left turn signal (CH11) | Front left | Rear left |
| Right turn signal (CH12) | Front right | Rear right |
| Fan (CH2) | 1 unit, engine bay | — |
| Starter relay (CH5) | Relay + power cable | — |
| Alternator | Charge cable | — |
| Stop lights (Stop Pedal) | — | Rear stop lights (L+R separate, via splice s-6) |
| Fuel pump (CH3) | — | Fuel pump |

Both harnesses receive power from the central panel. Channels CH10/CH11/CH12 split at the panel: one branch goes to the front harness, the other to the rear.

## Open Questions

1. **Automatic fan control** — CH2 Toggle = manual control only. For automatic temperature-based operation, a thermo-switch and control circuit are needed. Decision: add a separate thermo-relay in parallel with CH2, or keep manual control only.
2. **STA signal to ECU** — confirm whether to take the signal from CH5 (before relay) or from the relay power output (after relay). From CH5 — ECU "sees" the starter command even if the relay fails (false signal). From relay output — signal is reliable but requires an extra wire from the relay.
3. **Route lengths** — shown as approximate. Requires measurement after final component placement and routing is determined.
4. **Fan power rating** — confirm model and actual current draw to verify 2.5 mm² gauge. If current exceeds 20A, 4 mm² may be needed.
