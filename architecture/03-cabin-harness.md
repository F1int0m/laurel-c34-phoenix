# Cabin Harness — X300

## 1. Purpose and Routing

The cabin harness connects the central panel to cabin consumers: heater motor, wiper motor, OBD port, and the power bus. From MICTUNING, **only +12V wires** run into the harness. Each consumer's ground goes through dedicated GND pins in connector X300; on the mating side, each GND pin connects to the nearest body ground point with a short wire (200–400 mm).

**Route:** from the central panel behind the glove box, down to cabin components — heater motor, wiper mechanism, OBD port. The harness runs diagonally through the cabin, hidden under trim panels.

**Stop Pedal (C4):** the stop light switch is a standalone component — not part of the cabin harness and not routed through X300. Powered directly from main bus+ (constant +12V). Pin 1 (In) ← main bus+; Pin 2 (Out) → splice s-6 → X200.5/X200.6 (rear stop lights).

```
Panel → X300 (cabin harness) → HeaterMotor (heater motor, CH7)
                               → WiperMotor (wiper motor, CH9)
                               → OBD (OBD-II, powered from IGN bus CH1)

Stop Pedal (C4) ← main bus+ → splice s-6 → X200 (rear stop lights)
```

The power topology (battery, kill switch, main bus+) is described in [[01-central-panel]].

## 2. MICTUNING Channels Through Cabin Harness

| Channel | Fuse | Mode | Function | Load |
|---------|------|------|----------|------|
| CH7 | 30A | Toggle | Heater (motor) | Stock Laurel motor, ~15–20A |
| CH9 | 20A | Toggle | Wipers (motor) | Stock Laurel motor, ~5–10A |

CH6 is no longer used for stop lights — the stop pedal switch (C4) is powered directly from main bus+, not through MICTUNING.

## 3. Connector X300 — Pinout

Connector X300 is 6-pin (3 signal + 3 GND). Each consumer has a dedicated GND pin; on the mating side, GND pins connect to body ground points with short wires (200–400 mm).

| Pin | Signal | Goes to | Note |
|-----|--------|---------|------|
| 1 | +12V (MICT CH7) | Heater motor (+) | 30A fuse, Toggle |
| 2 | +12V (MICT CH9) | Wiper motor (+) | 20A fuse, Toggle |
| 3 | +12V IGN bus (CH1) | OBD port (+) | OBD powered from IGN bus |
| 4 | GND heater motor | Body ground point (X300 mating side) | 2.5 mm², short wire 200–400 mm |
| 5 | GND wiper motor | Body ground point (X300 mating side) | 1.5 mm², short wire 200–400 mm |
| 6 | GND OBD port | Body ground point (X300 mating side) | 0.75 mm², short wire 200–400 mm |

**Stop light:** the stop pedal switch (C4) is a standalone component, not routed through X300. Powered from main bus+ directly, via splice s-6 to X200.5/X200.6.

## 4. Cabin Harness Circuit Table

### +12V Circuits (from MICTUNING through harness)

| ID | From | To | Length (mm) | Gauge (mm²) | Color | Fuse | MICT Ch | Note |
|----|------|----|-------------|-------------|-------|------|---------|------|
| C01 | MICT CH7 | Heater motor (+) | ~800 | 2.5 | red | 30A | CH7 | Heater motor power (Toggle) |
| C03 | MICT CH9 | Wiper motor (+) | ~600 | 1.5 | blue | 20A | CH9 | Wiper motor power (Toggle) |
| C07 | IGN bus (CH1) | OBD port (+) | ~500 | 0.75 | green | 5A | CH1 | OBD powered from IGN bus |

### Ground Circuits (GND pins X300 → body)

Ground wires do not run through the harness. On the X300 mating side, each GND pin connects to the nearest body ground point with a short wire. Body ground points connect to a common point (star topology).

| ID | Consumer | X300 GND pin | Gauge (mm²) | Color | Length (mm) | Note |
|----|----------|-------------|-------------|-------|-------------|------|
| C02 | Heater motor | Pin 4 | 2.5 | black | 200–400 | GND pin → body ground point |
| C04 | Wiper motor | Pin 5 | 1.5 | black | 200–400 | GND pin → body ground point |
| C08 | OBD port | Pin 6 | 0.75 | black | 200–400 | GND pin → body ground point |

## 5. Connection Descriptions

### Heater Motor
- Power: +12V from MICT CH7 (30A, Toggle) through X300 pin 1
- Wire gauge 2.5 mm² (current ~15–20A)
- Ground: GND pin X300 pin 4 → body ground point (wire 2.5 mm², 200–400 mm)

### Wiper Motor
- Power: +12V from MICT CH9 (20A, Toggle) through X300 pin 2
- Wire gauge 1.5 mm² (current ~5–10A)
- Ground: GND pin X300 pin 5 → body ground point (wire 1.5 mm², 200–400 mm)

### Stop Light (Brake Light)
- Stop pedal switch (C4) — standalone 2-pin component, not routed through X300 or cabin harness
- Circuit: main bus+ (constant +12V) → Stop Pedal pin 1 (In) → Stop Pedal pin 2 (Out) → splice s-6 → X200.5/X200.6 (rear stop lights)
- Wire gauge 1.0 mm² (21W bulbs each)
- Switch is normally open, closes when pedal is pressed
- Stop lights work regardless of ignition state (powered from main bus+, not through MICT CH6)
- CH6 is a free reserve channel
- Stop light grounds connect in the rear harness (X200), not in the cabin harness

### OBD-II Port
- Power: from IGN bus (CH1) through 5A fuse (C07)
- Wire gauge 0.75 mm²
- Ground: GND pin X300 pin 6 → body ground point (wire 0.75 mm², 200–400 mm)
- CAN bus connects to ECU for digital dash

### Kill Switch and Main Bus+

Described in [[01-central-panel]].

## 6. Wire Gauges and Rationale

| Consumer | Gauge (mm²) | Rationale |
|----------|-------------|-----------|
| Heater motor (+/GND) | 2.5 | Current ~15–20A, short route |
| Wiper motor (+/GND) | 1.5 | Current ~5–10A |
| Stop light | 1.0 | 21W bulbs, separate circuit from Stop Pedal (not through X300) |
| OBD port (+/GND) | 0.75 | Diagnostic, low-current circuit |

GND pin gauge = gauge of the corresponding + wire.

## 7. What Is NOT in the Cabin Harness

- Interior light — not needed
- A/C — removed
- Column stalk switches — no, all control through MICTUNING
