# Central Distribution Unit (MICTUNING PDM)

## 1. System Architecture Overview

The system is built around the MICTUNING P1B — a PDM (Power Distribution Module) that replaces the traditional fuse and relay box. Each PDM channel has its own fuse and can deliver power directly to the load. External relays are used only for the starter (low-current control).

### Power Topology

```
Battery (trunk)
    │
    │ 50 mm² (+)
    │
  Kill switch (positive line)
    │
    │ 50 mm² (+)
    │
  Main bus+ (cabin)
    │
    ├──→ MICTUNING PDM (80A cable) ──→ 12 channels ──→ Loads
    │
    ├──→ Alternator (80A cable, charge)
    │
    └──→ Starter relay ──→ 50 mm² ──→ Starter

  ECU BATT: from bus+ via separate 10A fuse (not through MICT)

  MICTUNING: ground through housing (metal body mount)

  Ground buses: engine, cabin, trunk — each directly to common point (star)
```

### Principle: MICTUNING as PDM

MICTUNING is a **Power Distribution Module**, not just a switch panel. Each channel:

- Has its **own fuse** (circuit protection is built into the channel)
- Can **deliver power directly to the load** (up to the channel fuse rating)
- Does not require external relays for loads up to 30A

External relays are only needed when:
- Load exceeds the channel fuse rating
- Galvanic isolation is required (starter — low-current relay control)

## 2. MICTUNING P1B — Specifications

- **Model:** 12 gang (80A total current, 960W at 12V)
- **Voltage:** 12V DC
- **Protection:** IP65
- **Power:** direct from main bus+ (80A cable)
- **Ground:** through housing (metal body mount)
- **4 control modes:**
  - Red — Toggle (on/off, constant power)
  - Green — Momentary (held)
  - Blue — Flash (intermittent, for turn signals)
  - Cyan — Pulsed Strobe (for hazard lights)

## 3. Channels and Fuses — Approved Allocation

| CH | Fuse | Function | Mode | Note |
|----|------|----------|------|------|
| 1 | 30A | IGN bus (ECU +12V) | Toggle | Powers ignition and ECU BATT |
| 2 | 30A | Cooling fan | Toggle | Direct power from PDM |
| 3 | 20A | Fuel pump | Toggle | Direct power from PDM |
| 4 | 15A | Low beam (halogen) | Toggle | Left + right headlights in parallel |
| 5 | 10A | Starter relay | Momentary | Low-current relay coil control |
| 6 | 10A | Reserve | — | Free channel |
| 7 | 30A | Heater | Toggle | Direct power from PDM |
| 8 | 30A | Reserve | — | High-current channel |
| 9 | 20A | Wipers | Toggle | Direct power from PDM |
| 10 | 15A | Parking lights (front + rear) | Toggle | All parking lights in parallel |
| 11 | 10A | Left turn signal | Flash | Front + rear on same side |
| 12 | 10A | Right turn signal | Flash | Front + rear on same side |

### Channel Notes

**CH1 — IGN bus (ECU):** supplies +12V to the engine ECU.

**CH5 — Starter relay:** Momentary mode — power is supplied only while the button is held. CH5 provides +12V to the coil of the external starter relay, which switches high-current +50 mm² from main bus to starter.

**CH6 — Reserve:** channel is free. Stop lights are powered directly from main bus+ through the pedal switch (Stop Pedal, C4), not through MICTUNING.

**CH11/CH12 — Turn signals:** Flash mode generates the intermittent signal. Each channel drives front + rear turn signals on one side.

## 4. Power Circuits: Battery, Kill Switch, Buses

### Battery and Main Bus+

The battery is trunk-mounted. A power cable runs through a kill switch on the positive line to the main bus+ in the cabin.

| Circuit | Gauge (mm²) | Color | Note |
|---------|-------------|-------|------|
| Battery (+) → Kill switch | 50 | Red | Positive line |
| Kill switch → Main bus+ | 50 | Red | Positive line |
| Main bus+ → MICTUNING | 80A cable | Red | PDM power |
| Main bus+ → Alternator | 80A cable | Red | Charge cable |
| Main bus+ → Starter relay | 50 | Red | Starter high-current power |
| Starter relay → Starter | 50 | Red | Cranking current |
| Main bus+ → 10A fuse → ECU BATT | 0.75 | Red | Not through MICT |

### Kill Switch

Installed on the **positive line** between battery and main bus+. Disconnects all +12V in the system. Rating 100–200A.

### Starter Relay

The only external relay in the system. Switches high-current +50 mm² from main bus to starter. Relay coil is driven by a low-current signal from MICTUNING CH5 (Momentary).

## 5. Ground Scheme — Star Topology

The design uses distributed ground buses, each connected **directly to a common point (star)** rather than in series (daisy chain). This eliminates floating ground issues and simplifies diagnostics.

### Ground Buses

- **Engine** — grounds for engine, alternator, fan, starter. Heavy wire from engine point to common point.
- **Cabin** — grounds for MICTUNING (through housing), heater, wipers, OBD, ECU BATT.
- **Trunk** — grounds for fuel pump, tail lights.

Each bus is a separate wire directly to the common point (body/battery−). No daisy-chaining between buses.

### MICTUNING: Ground Through Housing

MICTUNING grounds through its metal body mount. No additional ground wire is required.

## 6. MICTUNING Power Supply and OBD

| Circuit | Gauge (mm²) | Color | Note |
|---------|-------------|-------|------|
| Main bus+ → MICTUNING | 80A cable | Red | PDM power |
| MICTUNING ground | — | — | Through housing (body mount) |
| Main bus+ → 5A fuse → OBD-II | 0.75 | Yellow | Diagnostic port power (from IGN bus CH1) |
| Common ground point → OBD-II | 0.75 | Black | OBD ground |
| Main bus+ → 10A fuse → ECU BATT | 0.75 | Red | Constant ECU power (not through MICT) |

## 7. BOM — Bill of Materials (Central Panel)

| # | Item | Size/type | Qty | Note |
|---|------|-----------|-----|------|
| 1 | MICTUNING P1B 12-gang PDM | 80A, IP65 | 1 | Central PDM |
| 2 | Starter relay (30A contacts) | 4-pin | 1 | Low-current control from CH5 |
| 3 | Kill switch | 100–200A | 1 | On positive line |
| 4 | Main fuse (F_MAIN) | 150–200A, ANL | 1 | On battery → bus+ line |
| 5 | ANL fuse holder | MiniANL, 200A | 1 | For F_MAIN |
| 6 | 10A fuse (ECU BATT) | Mini | 1 | Separate, not through MICT |
| 7 | 5A fuse (OBD-II) | Mini | 1 | From IGN bus CH1 |
| 8 | 50 mm² wire | Red | 5 m | Battery→Kill switch→bus+, relay→starter |
| 9 | 50 mm² wire | Black | 2 m | Battery− → common ground point |
| 10 | 80A cable | Red | 3 m | Bus+→MICTUNING, bus+→alternator |
| 11 | 2.5 mm² wire | Red/Black | 10 m | Heater, fan, fuel pump (from MICT) |
| 12 | 1.5 mm² wire | Red | 10 m | Low beam (from MICT) |
| 13 | 1.0 mm² wire | Red | 5 m | Stop lights (from Stop Pedal to splice s-6 and X200) |
| 14 | 1.5 mm² wire | Red/Black | 5 m | Wipers (from MICT) |
| 15 | 0.75 mm² wire | Red | 20 m | Parking lights, turn signals, ECU IGN, STA, OBD |
| 16 | 0.75 mm² wire | Black | 15 m | Ground wires for low-current loads |
| 17 | 0.75 mm² wire | Yellow | 3 m | OBD-II power |
| 18 | 0.75 mm² wire | Black | 3 m | OBD-II ground |
| 19 | Copper bus bars (screw terminals) | + and − | 2 | Main bus+, common ground point |
| 20 | Local ground buses | 6–8 mm | 3 | Engine, cabin, trunk |
| 21 | O-ring terminals (to bus bar) | 6–8 mm | 15 | Under bus bar screws |
| 22 | Insulated spade terminals | 1.5–6 mm | 40 | Wire-to-terminal connections |
| 23 | Contacts (copper, 2.8–6.3 mm) | — | 50 | For connectors |
| 24 | Adhesive heat-shrink tubing (2:1 and 3:1) | Ø 1–10 mm | 1 pack | Crimp sealant |
| 25 | 3M Temflex 1700 tape | Blue | 1 | Harness insulation |
| 26 | PET braided sleeve | Ø 8–20 mm | 5 m | Harness protection |
| 27 | Plastic zip ties | 200×4.6 mm | 50 | Harness fixation |
| 28 | Contact cleaner spray | — | 1 | Corrosion prevention on terminals |
| 29 | OBD-II connector (female) | 16-pin | 1 | Diagnostic port |

Wire lengths include ~10% margin. Connector contacts are in 2–3x reserve.

## 8. Assembly Recommendations

### Crimp Connections

Use crimp terminals with adhesive heat-shrink tubing and a professional crimper. Soldering is prohibited — soldered joints are unreliable due to brittleness and corrosion under vibration. Always tug-test after crimping — wires must not pull out of the connector. Heat-shrink with adhesive (3:1) reliably seals the connection and protects against moisture.

### Labeling

Number or label every wire and harness. Use numbered heat-shrink or typewriter tape labels. Label according to the circuit diagram IDs from the tables. Critical for debugging.

### Moisture Protection

Harness entry points into the engine bay and cabin must use rubber grommets. All connectors in the engine bay must be waterproof (IP67) or additionally sealed. Wrap wires with 3M tape or use PET braided sleeve for mechanical protection.

### Fastening

Secure harnesses with plastic zip ties to rigid body elements every 50–100 mm. Avoid unsupported spans. Minimum bend radius — no less than 5–10 wire diameters.

### Materials

Use wire with FLRY or GXL insulation (for engine bay — with additional fluorosilicone coating). Connectors — factory-sealed (Deutsch, AMP, Nissan OEM). Torque all terminals to specification.

### Grounds — Critical

Each local ground bus (engine, cabin, trunk) connects **via a separate wire directly to the common point** (star topology). Daisy-chaining ground buses is prohibited — it causes floating grounds and hard-to-diagnose problems. MICTUNING grounds through its housing (body mount).

### Finished Harness Testing

Before installation — verify for short circuits and conformance to the schematic. Fill each connector cavity with inspection paste for visual confirmation of contact seating.
