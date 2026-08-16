# ECU Wiring

## 1. Purpose

Connecting the Toyota 1UZ VVT-i engine control unit (ECU) to the project wiring system. The ECU-to-engine wiring (sensors, injectors, coils, MAF, O2, etc.) is already complete — it's the stock Toyota harness. The project only needs to supply power to the ECU and provide starter signals.

## 2. Connector X400 — ECU Harness

The ECU harness is a standalone harness with its own **X400** connector. Power is taken **from the MICTUNING PDM** — the PDM and ECU are physically close, so +12V for IGN (CH1) and BATT are taken locally from the PDM/main bus in the PDM area, not from the central panel. The harness carries **only +12V wires**. No ground wires are routed through the harness back. At the harness end, connector X400 has **dedicated GND pins** in addition to the + pins — one per consumer. On the mating side of the connector, these GND pins connect to the nearest body ground point with short wires (200–400 mm).

### X400 Pinout

| X400 Pin | Signal | Direction | Connects To | Gauge (mm²) | Note |
|----------|--------|-----------|-------------|-------------|------|
| 1 | +12V IGN | → (into harness) | ECU (pin IGSW/E1) | 0.75 | From MICT CH1 (30A, Toggle) |
| 2 | +12V BATT | → (into harness) | ECU (pin BATT) | 0.75 | From main bus+ (fuse 10A), bypassing MICT |
| 3 | STA | → (into harness) | ECU (pin STA) | 0.75 | From CH5 or starter relay output |
| 4 | GND IGN | ← (to body) | Body ground point | 0.75 | On X400 mating side — to nearest ground point |
| 5 | GND BATT | ← (to body) | Body ground point | 0.75 | On X400 mating side — to nearest ground point |

**Principle:** +12V wires (pins 1–3) are taken from the PDM/main bus in the MICTUNING area and run through the harness to the ECU. GND pins (4–5) are dedicated ground points: on the X400 mating side they connect to the nearest body ground point with short wires (200–400 mm).

## 3. ECU Circuits — Only What the Project Connects

### ECU Power

| Circuit | Signal | From | To | Gauge (mm²) | Fuse | MICT Channel | Via X400 | Note |
|---------|--------|------|----|-------------|------|--------------|----------|------|
| +12V IGN | Ignition | MICT CH1 (30A, Toggle) | ECU (pin IGSW/E1) | 0.75 | 30A (CH1) | CH1 | Pin 1 | Powers on the ECU |
| +12V BATT | Constant memory power | Main bus+ (separate 10A fuse) | ECU (pin BATT) | 0.75 | 10A (separate) | — | Pin 2 | Direct from bus, not through MICT |

**BATT** — constant power for ECU memory retention (codes, adaptive values). Connected directly from main bus+ through a separate 10A fuse, bypassing MICTUNING. This ensures the ECU retains memory even when MICT channels are depowered. The +12V BATT wire passes through connector X400 (pin 2), and its GND goes through X400 pin 5 to the body.

### Starter

| Circuit | Signal | From | To | Gauge (mm²) | Fuse | MICT Channel | Via X400 | Note |
|---------|--------|------|----|-------------|------|--------------|----------|------|
| Starter relay coil | +12V (Momentary) | MICT CH5 (10A, Momentary) | Starter relay (coil) | 0.75 | 10A (CH5) | CH5 | — | While button is held |
| STA signal to ECU | +12V (start) | CH5 or relay output | ECU (pin STA) | 0.75 | — | — | Pin 3 | ECU knows starter is engaged |

**Operating principle:**
1. CH5 (Momentary) supplies +12V to the starter relay coil
2. Starter relay switches high-current + from main bus to starter (50 mm² power circuit — see 05-front-harness)
3. ECU receives STA signal that the starter is engaged — from the same CH5 or from the relay power output

### Fuel Pump

Fuel pump power is supplied directly from MICT CH3 (20A, Toggle) — see [[02-rear-harness]], circuit P18. The ECU does not control the fuel pump.

### Alternator

Signal L from alternator to ECU — if required, but this is already in the stock Toyota wiring. No separate circuit needed from the project.

## 4. ECU Ground

ECU power grounds (IGN, BATT) are now provided through dedicated GND pins on connector X400:

- **GND IGN** (X400 pin 4) — ignition circuit ground. On the mating side, connects to the nearest body ground point (200–400 mm).
- **GND BATT** (X400 pin 5) — constant power circuit ground. On the mating side, connects to the nearest body ground point (200–400 mm).

ECU sensor and case grounds are provided by the **stock Toyota wiring** — mounting to body via factory ground points. This is unchanged.

**Summary:**
- Power grounds (IGN, BATT) → through X400 GND pins → body (project)
- Sensor / case grounds → stock Toyota wiring (unchanged)

## 5. What Is NOT Described in This File

The following circuits are already complete in the stock Toyota wiring and require no design:

- Sensors (MAF, O2, CKP, CMP, ECT, IAT, TPS, KNK, etc.)
- Injectors
- Ignition coils
- VVT-i solenoids
- MREL, FC, FPR signals from ECU — not used (fuel pump from MICT)
- OR diode circuits for fuel pump — not needed

## 6. ECU Connections Summary from the Project

### Signal (+) Connections via X400

| X400 Pin | ECU Pin | Signal | Source | Gauge |
|----------|---------|--------|--------|-------|
| 1 | IGSW/E1 | +12V IGN | MICT CH1 | 0.75 mm² |
| 2 | BATT | +12V constant | Main bus+ (fuse 10A) | 0.75 mm² |
| 3 | STA | Starter signal | CH5 or relay output | 0.75 mm² |

### X400 GND Pins (to body on mating side)

| X400 Pin | Signal | To | Gauge |
|----------|--------|----|-------|
| 4 | GND IGN | Body ground point (200–400 mm) | 0.75 mm² |
| 5 | GND BATT | Body ground point (200–400 mm) | 0.75 mm² |

Fuel pump (CH3) — not an ECU circuit, see [[02-rear-harness]].

Everything else — stock Toyota wiring, already complete.
