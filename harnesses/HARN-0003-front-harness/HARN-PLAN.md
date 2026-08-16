# Build Plan — HARN-0003 Front Harness

**Source brief:** harnesses/HARN-0003-front-harness/README.md
**Generated:** 2026-08-15
**Target harness:** existing K1vY (Laurel harness)
**Strategy:** incremental — расширение существующего документа

## Build Order

```
Parts (connectorParts, contactParts, wireParts)
  → Phase 1: Panel-side X100 expansion (14→20 cav)
    → Phase 2: Front-side connector + mate
      → Phase 3: Device connectors
        → Phase 4: Front harness wiring
          → Phase 5: Bundles + layout
```

## Phase 1 — Panel-side X100 Expansion (14→20 cavities)

X100 (c-x100) сейчас 14-пиновый (cp-x100). Необходимо расширить до 20 пинов для 7 GND + резерв.

### 1.1 Parts

| Part type | ID | Part number | Spec | Notes |
|-----------|----|-------------|------|-------|
| connectorPart | cp-x100-20 | DT-20-P | 20 cav, shell, Male | Заменяет cp-x100 (14 cav) |
| contactPart | kpt-dt-20 | DT-20-pin | Male pin | Для новых cavities X100 |

### 1.2 X100 Expansion

| Operation | Details |
|-----------|---------|
| update c-x100 | partId → cp-x100-20 |
| add cavities | x100-15, x100-16, x100-17, x100-18, x100-19, x100-20 (6 новых) |
| update signals | x100-4 → "CH5_Start", x100-9 → "IGN_ECU", x100-10 → "BATT_ECU", x100-11 → "STA_ECU", x100-12 → "GND_HeadL_Low", x100-13 → "GND_HeadR_Low", x100-14 → "GND_Fan", x100-15 → "GND_HeadL_Park", x100-16 → "GND_HeadR_Park", x100-17 → "GND_TurnL", x100-18 → "GND_TurnR", x100-19 → "NC", x100-20 → "NC" |

### 1.3 Panel-side wires to X100 (недостающие)

| Wire ID | From | To (c-x100.cavity) | Gauge | Color | Notes |
|---------|------|---------------------|-------|-------|-------|
| w-ch5-start | splice s-1 или MICT.5 | x100-4 | 0.75 | зелён. | CH5 → реле стартера |
| w-ign-ecu | splice s-1 (IGN) | x100-9 | 0.75 | красн. | IGN на ЭБУ |
| w-batt-ecu | t-bus-plus.Terminal | x100-10 | 0.75 | красн. | BATT ЭБУ (через FUSE_BATT_ECU) |
| w-sta-ecu | relay output или CH5 | x100-11 | 0.75 | оранж. | STA на ЭБУ |

**Примечание:** w-ch2-fan, w-head-l, w-head-r, w-park-lf, w-park-rf, w-turn-lf, w-turn-rf уже подключены к x100-1..3, x100-5..8.

## Phase 2 — Front-side Connector + Mate

### 2.1 Parts

| Part type | ID | Part number | Spec | Notes |
|-----------|----|-------------|------|-------|
| connectorPart | cp-x100-front | DT-20-S | 20 cav, shell, Female | Ответная часть X100 |
| contactPart | kpt-dt-20-s | DT-20-socket | Female socket | Для cp-x100-front |

### 2.2 Connector

| ID | Label | Part | Cavities | Shell | Position (schematic) |
|----|-------|------|----------|-------|---------------------|
| c-x100-front | X100-FRONT | cp-x100-front | 20 | Yes | Справа от c-x100 |

Cavities (id: signal):

| Cavity ID | Designation | Signal |
|-----------|-------------|--------|
| x100f-1 | 1 | +12V_CH2_Fan |
| x100f-2 | 2 | +12V_CH4_HeadL |
| x100f-3 | 3 | +12V_CH4_HeadR |
| x100f-4 | 4 | +12V_CH5_Start |
| x100f-5 | 5 | +12V_CH10_ParkL |
| x100f-6 | 6 | +12V_CH10_ParkR |
| x100f-7 | 7 | +12V_CH11_TurnL |
| x100f-8 | 8 | +12V_CH12_TurnR |
| x100f-9 | 9 | +12V_IGN_ECU |
| x100f-10 | 10 | +12V_BATT_ECU |
| x100f-11 | 11 | STA_ECU |
| x100f-12 | 12 | GND_HeadL_Low |
| x100f-13 | 13 | GND_HeadR_Low |
| x100f-14 | 14 | GND_Fan |
| x100f-15 | 15 | GND_HeadL_Park |
| x100f-16 | 16 | GND_HeadR_Park |
| x100f-17 | 17 | GND_TurnL |
| x100f-18 | 18 | GND_TurnR |
| x100f-19 | 19 | NC |
| x100f-20 | 20 | NC |

Shell: x100f-shell

### 2.3 Mate

| Source | Target | Mate ID |
|--------|--------|---------|
| c-x100 | c-x100-front | m-x100 |

## Phase 3 — Device Connectors

### 3.1 Parts

| Part type | ID | Part number | Spec | Notes |
|-----------|----|-------------|------|-------|
| connectorPart | cp-head-l | DT-4-S | 4 cav, shell, Female | Левая фара |
| connectorPart | cp-head-r | DT-4-S | 4 cav, shell, Female | Правая фара |
| connectorPart | cp-fan | DT-2-S | 2 cav, no shell, Female | Вентилятор |
| connectorPart | cp-starter | DT-2-S | 2 cav, no shell, Female | Реле стартера (управление) |
| connectorPart | cp-alt | DT-3-S | 3 cav, no shell, Female | Генератор |
| connectorPart | cp-ecu-ign | DT-1-S | 1 cav, no shell, Female | ЭБУ IGSW |
| connectorPart | cp-ecu-batt | DT-1-S | 1 cav, no shell, Female | ЭБУ BATT |
| contactPart | kpt-dt-4 | DT-4-socket | Female socket | 4-pin фарный |
| contactPart | kpt-dt-2 | DT-2-socket | Female socket | 2-pin клемма |
| contactPart | kpt-dt-3 | DT-3-socket | Female socket | 3-pin |
| contactPart | kpt-dt-1 | DT-1-socket | Female socket | 1-pin клемма |

### 3.2 Connectors

| ID | Label | Part | Cavities | Shell | Schematic position |
|----|-------|------|----------|-------|-------------------|
| c-head-l | HEAD-L | cp-head-l | 4 | Yes | Подкапот, слева |
| c-head-r | HEAD-R | cp-head-r | 4 | Yes | Подкапот, справа |
| c-fan | FAN | cp-fan | 2 | No | Подкапот, центр |
| c-starter | STARTER | cp-starter | 2 | No | Подкапот, рядом с реле |
| c-alt | ALT | cp-alt | 3 | No | Подкапот, генератор |
| c-ecu-ign | ECU-IGN | cp-ecu-ign | 1 | No | Рядом с ЭБУ |
| c-ecu-batt | ECU-BATT | cp-ecu-batt | 1 | No | Рядом с ЭБУ |

#### c-head-l cavities

| Cavity ID | Designation | Signal |
|-----------|-------------|--------|
| hl-1 | 1 | +12V_Ближний |
| hl-2 | 2 | +12V_Габарит |
| hl-3 | 3 | +12V_Поворотник |
| hl-4 | 4 | GND |

Shell: hl-shell

#### c-head-r cavities

| Cavity ID | Designation | Signal |
|-----------|-------------|--------|
| hr-1 | 1 | +12V_Ближний |
| hr-2 | 2 | +12V_Габарит |
| hr-3 | 3 | +12V_Поворотник |
| hr-4 | 4 | GND |

Shell: hr-shell

#### c-fan cavities

| Cavity ID | Designation | Signal |
|-----------|-------------|--------|
| fan-1 | 1 | +12V_Fan |
| fan-2 | 2 | GND_Fan |

#### c-starter cavities

| Cavity ID | Designation | Signal |
|-----------|-------------|--------|
| str-1 | 1 | +12V_Coil (CH5) |
| str-2 | 2 | STA_ECU |

#### c-alt cavities

| Cavity ID | Designation | Signal |
|-----------|-------------|--------|
| alt-1 | 1 | B/S (зарядный) |
| alt-2 | 2 | IG (управление) |
| alt-3 | 3 | L (лампа заряда) |

#### c-ecu-ign cavities

| Cavity ID | Designation | Signal |
|-----------|-------------|--------|
| ecu-ign-1 | 1 | +12V_IGN |

#### c-ecu-batt cavities

| Cavity ID | Designation | Signal |
|-----------|-------------|--------|
| ecu-batt-1 | 1 | +12V_BATT |

### 3.3 GND Terminals (точки массы кузова)

| ID | Label | Type | Signal |
|----|-------|------|--------|
| t-gnd-head-l | GND HeadL | Ring | GND_HeadL_Low |
| t-gnd-head-r | GND HeadR | Ring | GND_HeadR_Low |
| t-gnd-fan | GND Fan | Ring | GND_Fan |
| t-gnd-head-l-park | GND HeadL Park | Ring | GND_HeadL_Park |
| t-gnd-head-r-park | GND HeadR Park | Ring | GND_HeadR_Park |
| t-gnd-turn-l | GND TurnL | Ring | GND_TurnL |
| t-gnd-turn-r | GND TurnR | Ring | GND_TurnR |

## Phase 4 — Front Harness Wiring

### 4.1 Existing wireParts (reuse from document)

| ID | Spec | Color |
|----|------|-------|
| wp-025-blu | 2.5 mm² | синий |
| wp-015-red | 1.5 mm² | красный |
| wp-015-blk | 1.5 mm² | чёрный |
| wp-075-red | 0.75 mm² | красный |
| wp-075-grn | 0.75 mm² | зелёный |
| wp-075-wht | 0.75 mm² | белый |
| wp-075-lbl | 0.75 mm² | голубой |
| wp-075-yel | 0.75 mm² | жёлтый |
| wp-075-org | 0.75 mm² | оранжевый |
| wp-075-blk | 0.75 mm² | чёрный |

### 4.2 +12V Wires (X100-front → Device connectors)

| Wire ID | From (id.handle) | To (id.handle) | Part | Color | Notes |
|---------|------------------|----------------|------|-------|-------|
| fw-ch2-fan | c-x100-front.x100f-1 | c-fan.fan-1 | wp-025-blu | синий | Вентилятор |
| fw-head-l | c-x100-front.x100f-2 | c-head-l.hl-1 | wp-015-red | красн. | Ближний левый |
| fw-head-r | c-x100-front.x100f-3 | c-head-r.hr-1 | wp-015-blk | чёрн. | Ближний правый |
| fw-ch5-start | c-x100-front.x100f-4 | c-starter.str-1 | wp-075-grn | зелён. | Катушка реле |
| fw-park-lf | c-x100-front.x100f-5 | c-head-l.hl-2 | wp-075-wht | бел. | Габарит левый |
| fw-park-rf | c-x100-front.x100f-6 | c-head-r.hr-2 | wp-075-lbl | голуб. | Габарит правый |
| fw-turn-lf | c-x100-front.x100f-7 | c-head-l.hl-3 | wp-075-yel | жёлт. | Поворот левый |
| fw-turn-rf | c-x100-front.x100f-8 | c-head-r.hr-3 | wp-075-grn | зелён. | Поворот правый |
| fw-ign-ecu | c-x100-front.x100f-9 | c-ecu-ign.ecu-ign-1 | wp-075-red | красн. | IGN на ЭБУ |
| fw-batt-ecu | c-x100-front.x100f-10 | c-ecu-batt.ecu-batt-1 | wp-075-red | красн. | BATT ЭБУ |
| fw-sta-ecu | c-x100-front.x100f-11 | c-starter.str-2 | wp-075-org | оранж. | STA на ЭБУ |

### 4.3 GND Wires (X100-front → Body ground terminals)

| Wire ID | From (id.handle) | To (id.handle) | Part | Color | Notes |
|---------|------------------|----------------|------|-------|-------|
| gnd-head-l-low | c-x100-front.x100f-12 | t-gnd-head-l.Terminal | wp-015-blk | чёрн. | GND фара L ближний |
| gnd-head-r-low | c-x100-front.x100f-13 | t-gnd-head-r.Terminal | wp-015-blk | чёрн. | GND фара R ближний |
| gnd-fan | c-x100-front.x100f-14 | t-gnd-fan.Terminal | wp-025-blk | чёрн. | GND вентилятор |
| gnd-head-l-park | c-x100-front.x100f-15 | t-gnd-head-l-park.Terminal | wp-075-blk | чёрн. | GND фара L габарит |
| gnd-head-r-park | c-x100-front.x100f-16 | t-gnd-head-r-park.Terminal | wp-075-blk | чёрн. | GND фара R габарит |
| gnd-turn-l | c-x100-front.x100f-17 | t-gnd-turn-l.Terminal | wp-075-blk | чёрн. | GND поворот L |
| gnd-turn-r | c-x100-front.x100f-18 | t-gnd-turn-r.Terminal | wp-075-blk | чёрн. | GND поворот R |

## Phase 5 — Bundles + Layout

### 5.1 Branch Points

| ID | Layout Position | Notes |
|----|----------------|-------|
| bp-front | (12, -15) | Разветвление передней косы |

### 5.2 Bundles

| ID | From | To | Length (mm) | Wires | Notes |
|----|------|----|----|-------|-------|
| b-front-main | c-x100-front | bp-front | 300 | Все fw-* провода | Основной жгут от X100 до разветвления |
| b-head-l | bp-front | c-head-l | 400 | fw-head-l, fw-park-lf, fw-turn-lf | К левой фаре |
| b-head-r | bp-front | c-head-r | 800 | fw-head-r, fw-park-rf, fw-turn-rf | К правой фаре |
| b-fan | bp-front | c-fan | 600 | fw-ch2-fan | К вентилятору |
| b-starter | bp-front | c-starter | 300 | fw-ch5-start, fw-sta-ecu | К реле стартера |
| b-ecu | bp-front | c-ecu-ign | 400 | fw-ign-ecu, fw-batt-ecu | К ЭБУ |
| b-gnd-head-l-low | c-x100-front | t-gnd-head-l | 300 | gnd-head-l-low | GND на кузов |
| b-gnd-head-r-low | c-x100-front | t-gnd-head-r | 300 | gnd-head-r-low | GND на кузов |
| b-gnd-fan | c-x100-front | t-gnd-fan | 300 | gnd-fan | GND на кузов |
| b-gnd-head-l-park | c-x100-front | t-gnd-head-l-park | 200 | gnd-head-l-park | GND на кузов |
| b-gnd-head-r-park | c-x100-front | t-gnd-head-r-park | 200 | gnd-head-r-park | GND на кузов |
| b-gnd-turn-l | c-x100-front | t-gnd-turn-l | 200 | gnd-turn-l | GND на кузов |
| b-gnd-turn-r | c-x100-front | t-gnd-turn-r | 200 | gnd-turn-r | GND на кузов |

## Cross-Module Connections

| Phase | Connector | Phase | Connector | Shared circuits |
|-------|-----------|-------|-----------|----------------|
| Central Panel | c-x100 | Front Harness | c-x100-front | CH2, CH4, CH5, CH10, CH11, CH12, IGN, BATT, STA, 7×GND |

## ALT Exception

Генератор ALT — B/S (cavity alt-1) подключается **напрямую к шине+** отдельным кабелем 6-10 мм², не через X100. В harness.design это будет отдельный провод от t-bus-plus.Terminal к c-alt.alt-1. Кавалти alt-2 (IG) и alt-3 (L) — TBD (зависит от схемы генератора и приборки).

## Verification Checklist

- [ ] cp-x100-20: numberOfCavities == 20, hasShell == true
- [ ] c-x100: 20 cavities, partId == cp-x100-20
- [ ] c-x100-front: 20 cavities, partId == cp-x100-front, Female
- [ ] Mate m-x100: c-x100 ↔ c-x100-front
- [ ] Все +12V провода fw-* соединяют c-x100-front.cavity → device.cavity
- [ ] Все GND провода gnd-* соединяют c-x100-front.cavity → terminal.Terminal
- [ ] Panel-side: x100-4, x100-9..11 подключены к соответствующим цепям на панели
- [ ] Нет duplicate IDs
- [ ] Все connectorPart cavity counts == фактическое кол-во cavities

## Estimated Scope

- New connectorParts: 9
- New contactParts: 5
- New connectors: 8 (c-x100-front + 7 device)
- New terminals: 7 (GND points)
- New wires: 18 (11 power + 7 GND)
- New mate: 1
- New bundles: ~12
- X100 expansion: +6 cavities + 1 new part
- Operations: ~40-50 (edit_document)

## Risks

| Risk | Mitigation |
|------|------------|
| X100 expansion может нарушить существующие wires | dryRun перед выполнением; существующие wires используют cavity IDs, которые не меняются |
| Panel-side wiring для x100-4, x100-9..11 ещё не подключена | Добавить в рамках этого плана (Phase 1.3) |
| ALT B/S кабель не через X100 — необычная схема | Отдельный провод t-bus-plus → c-alt.alt-1 |
| LED-фары: общий GND или отдельные GND на цепь? | Поbrief — отдельные GND (7 шт.), но если LED-блок внутренне объединяет GND, потребуется ревизия |
