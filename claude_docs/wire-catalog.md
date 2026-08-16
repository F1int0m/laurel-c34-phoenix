# Wire & Parts Catalog — Laurel Harness

> **IMPORTANT:** Before using this catalog, verify that the part IDs and
> partNumbers match what is currently in the harness.design document.
> Run `get_document_json(fields=["wireParts","connectorParts",
> "contactParts","coveringParts"])` once and compare. If discrepancies
> exist, update this catalog.

## Wire Gauge Reference

| Gauge (mm2) | Typical Use | harness.design gauge value |
|-------------|-------------|---------------------------|
| 50 | Battery cables, starter power | 50 |
| 10 | Alternator charge cable / BUS+ to MICT BATT | 10 |
| 2.5 | High-current (heater, fan, fuel pump, main grounds) | 2.5 |
| 1.5 | Medium loads (headlights, wiper) | 1.5 |
| 1.0 | Stop lights | 1.0 |
| 0.75 | Low-current (parking, turn signals, ECU signals, OBD) | 0.75 |

## Color Naming Convention

### Russian to English to harness.design PartColor

| Russian (abbrev.) | English | harness.design value |
|--------------------|---------|---------------------|
| красн. | Red | Red |
| синий | Blue | Blue |
| чёрн. | Black | Black |
| зелён. | Green | Green |
| бел. | White | White |
| голуб. | Light Blue | Light Blue |
| жёлт. | Yellow | Yellow |
| оранж. | Orange | Orange |
| фиолет. | Violet | Violet |

### Stripe colors

stripeColor — separate field, uses same values as color.

## Part ID Conventions

### wirePart: `wp-{gauge}`

Wire part ID contains only the gauge. Color is set per-wire in the
`color` field of each wire — not on the part. This keeps the part
library small (one entry per gauge) and lets any wire of that gauge
use any color.

Gauge encoding: 075 = 0.75mm2, 010 = 1.0mm2, 015 = 1.5mm2, 025 = 2.5mm2, 060 = 6.0mm2, 080 = 10mm2, 100 = 10mm2 (alt), 500 = 50mm2

| ID | gauge (mm2) | partNumber | Notes |
|----|-------------|------------|-------|
| wp-500 | 50 | WIRE-50 | Battery cables, starter |
| wp-080 | 10 | CABLE-80A | BUS+ to MICT BATT (80A) |
| wp-025 | 2.5 | WIRE-25 | High-current loads |
| wp-015 | 1.5 | WIRE-15 | Medium loads |
| wp-010 | 1.0 | WIRE-10 | Stop lights |
| wp-075 | 0.75 | WIRE-075 | Low-current signals |

> **Migration note:** Central panel (K1vY) still uses per-color IDs
> (`wp-025-red`, `wp-075-wht`, etc.). When editing that document,
> migrate wireParts to the gauge-only scheme above and set color on
> each wire. The rear harness (jYky) already follows the new convention.

### connectorPart: `cp-{name}`

| ID | partNumber | manufacturer | gender | numberOfCavities | hasShell | contactPartId |
|----|------------|-------------|--------|------------------|----------|---------------|
| cp-mict | MICTUNING-P1B | MICTUNING | Female | 13 | false | kpt-mict |
| cp-x200-12 | DT-12-P | Deutsch | Male | 12 | true | kpt-dt-16 |
| cp-x300 | DT-8-P | Deutsch | Male | 8 | true | kpt-dt-8 |
| cp-x400 | DT-4-P | Deutsch | Male | 4 | true | kpt-dt-14 |
| cp-relay | BOSCH-ISO-4 | Bosch | Female | 4 | false | kpt-relay |
| cp-kill | KILL-SWITCH-2 | — | Female | 2 | false | kpt-kill |
| cp-tail-l | 4-pin combo | — | Female | 4 | true | kt-tail |
| cp-fuel | 2-pin terminal | — | Female | 2 | false | kt-fuel |
| cp-head-r | DT-4-S | — | Female | 4 | true | kpt-dt-4 |
| cp-fan | DT-2-S | — | Female | 2 | false | kpt-dt-2 |

### contactPart: `kpt-{name}` or `kt-{name}`

| ID | partNumber | gender | type | minGauge | maxGauge |
|----|------------|--------|------|----------|----------|
| kpt-dt-14 | DT-14-socket | Socket | Crimp | 0.5 | 2.5 |
| kpt-dt-8 | DT-8-socket | Socket | Crimp | 0.5 | 2.5 |
| kpt-dt-16 | DT-16-socket | Socket | Crimp | 0.5 | 2.5 |
| kpt-dt-20 | DT-20-pin | Pin | Crimp | 0.5 | 2.5 |
| kpt-dt-20-s | DT-20-socket | Socket | Crimp | 0.5 | 2.5 |
| kpt-dt-4 | DT-4-socket | Socket | Crimp | 0.5 | 2.5 |
| kpt-dt-2 | DT-2-socket | Socket | Crimp | 0.5 | 2.5 |
| kpt-dt-3 | DT-3-socket | Socket | Crimp | 0.5 | 2.5 |
| kpt-dt-1 | DT-1-socket | Socket | Crimp | 0.5 | 2.5 |
| kpt-relay | ISO-relay-socket | Socket | Crimp | 0.5 | 2.5 |
| kpt-mict | MICT-P1B-socket | Socket | Other | 0.5 | 50 |
| kpt-kill | KILL-socket | Socket | Other | 50 | 50 |
| kpt-fuse-mini | Mini-fuse-socket | Socket | Other | 0.5 | 2.5 |
| kpt-fuse-anl | ANL-fuse-socket | Socket | Other | 50 | 50 |
| kt-tail | TAIL-pin | Pin | Crimp | 0.5 | 1.0 |
| kt-fuel | FUEL-pin | Pin | Crimp | 2.5 | 2.5 |

### coveringPart: `cov-{name}`

| ID | type | partNumber | minDiameter | maxDiameter |
|----|------|------------|-------------|-------------|
| cov-pet | BraidedSleeve | PET-braid | 6 mm | 12 mm |
| cov-tape | Tape | 3M-Temflex-1700 | — | — |
