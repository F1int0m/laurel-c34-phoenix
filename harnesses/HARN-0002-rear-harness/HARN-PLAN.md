# Build Plan — HARN-0002 Rear Harness

**Source brief:** `harnesses/HARN-0002-rear-harness/README.md`
**Generated:** 2026-08-15
**Target harness:** new (создать в harness.design)
**Strategy:** single-phase — малый жгут (4 коннектора, 7 проводов)

## Build Order

```text
Parts (connectorParts, contactParts, wireParts)
  → Connectors + Cavities
    → Splice (s-6 для стоп-сигналов)
      → Wires
        → Bundles + Branch Points + Layout Points
          → Coverings
```

## Phase 1 — Rear Harness (единственная фаза)

### 1.1 Parts

| Part type | ID | Part number / spec | Cavity count / spec | Notes |
|-----------|----|--------------------|---------------------|-------|
| connectorPart | cp-x200 | Deutsch DT 16-pin | 16 cavities, shell | Инлайн к центральной панели |
| connectorPart | cp-tail-l | 4-pin combo | 4 cavities, shell | Левый LED-фонарь |
| connectorPart | cp-tail-r | 4-pin combo | 4 cavities, shell | Правый LED-фонарь |
| connectorPart | cp-fuel | 2-pin terminal | 2 cavities, no shell | Топливный насос |
| contactPart | kt-x200 | — | — | Контакты для X200 (0.75–2.5 мм²) |
| contactPart | kt-tail | — | — | Контакты для TAIL-L/R (0.75–1.0 мм²) |
| contactPart | kt-fuel | — | — | Контакты для FUEL (2.5 мм²) |
| wirePart | wp-075 | 0.75 мм² | — | Габариты, поворотники |
| wirePart | wp-10 | 1.0 мм² | — | Стоп-сигналы |
| wirePart | wp-25 | 2.5 мм² | — | Бензонасос |
| coveringPart | cov-pet | PET braid | Ø 8–20 мм | Основной жгут |
| coveringPart | cov-tape | 3M Temflex 1700 | — | Ветки к фонарям и насосу |

### 1.2 Connectors

| ID | Label | Part | Cavities | Shell | Schematic position | Notes |
|----|-------|------|----------|-------|-------------------|-------|
| c-x200 | X200 | cp-x200 | 16 | Yes | left-center | Инлайн-разъём к центральной панели |
| c-tail-l | TAIL-L | cp-tail-l | 4 | Yes | right-lower | Левый LED-фонарь |
| c-tail-r | TAIL-R | cp-tail-r | 4 | Yes | right-upper | Правый LED-фонарь |
| c-fuel | FUEL | cp-fuel | 2 | No | right-middle | Топливный насос |

#### Cavity assignments

**X200 (16 cavities):**

| Cavity ID | Signal | Gauge (mm²) | Color |
|-----------|--------|-------------|-------|
| x200-1 | +12V CH10 Park L | 0.75 | белый |
| x200-2 | +12V CH10 Park R | 0.75 | голубой |
| x200-3 | +12V CH11 L Turn | 0.75 | жёлтый |
| x200-4 | +12V CH12 R Turn | 0.75 | зелёный |
| x200-5 | +12V Stop L | 1.0 | фиолетовый |
| x200-6 | +12V Stop R | 1.0 | фиолетовый |
| x200-7 | +12V CH3 Fuel | 2.5 | красный |
| x200-8 | GND Park L | 0.75 | чёрный |
| x200-9 | GND Park R | 0.75 | чёрный |
| x200-10 | GND Turn L | 0.75 | чёрный |
| x200-11 | GND Turn R | 0.75 | чёрный |
| x200-12 | GND Stop L | 1.0 | чёрный |
| x200-13 | GND Stop R | 1.0 | чёрный |
| x200-14 | GND Fuel | 2.5 | чёрный |
| x200-15 | NC | — | — |
| x200-16 | NC | — | — |

**TAIL-L (4 cavities):**

| Cavity ID | Signal | Gauge (mm²) | Color |
|-----------|--------|-------------|-------|
| tl-1 | +12V Габарит | 0.75 | белый |
| tl-2 | +12V Стоп | 1.0 | фиолетовый |
| tl-3 | +12V Поворотник | 0.75 | жёлтый |
| tl-4 | GND | 1.0 | чёрный |

**TAIL-R (4 cavities):**

| Cavity ID | Signal | Gauge (mm²) | Color |
|-----------|--------|-------------|-------|
| tr-1 | +12V Габарит | 0.75 | голубой |
| tr-2 | +12V Стоп | 1.0 | фиолетовый |
| tr-3 | +12V Поворотник | 0.75 | зелёный |
| tr-4 | GND | 1.0 | чёрный |

**FUEL (2 cavities):**

| Cavity ID | Signal | Gauge (mm²) | Color |
|-----------|--------|-------------|-------|
| f-1 | +12V Бензонасос | 2.5 | красный |
| f-2 | GND | 2.5 | чёрный |

### 1.3 Splices

| ID | Circuits joined | Location | Notes |
|----|----------------|----------|-------|
| s-6 | Stop Pedal OUT → X200.5 + X200.6 | На центральной панели | Стоп-сигнал разветвляется на левый + правый |

**Примечание:** Splice s-6 физически находится на центральной панели. В задней косе мы создаём splice как точку ветвления, где один провод (Stop In) приходит от X200 и расходится на два провода к TAIL-L.2 и TAIL-R.2. Однако по архитектуре стоп-сигнал приходит уже разделённым на X200.5 и X200.6 — поэтому в самой задней косе splice не нужен: каждый стоп-провод идёт от своего cavity X200 напрямую к фонарю.

**Решение:** Splice s-6 находится на центральной панели (не в этом жгуте). В задней косе провода P17 и P17a идут напрямую от X200.5→TAIL-L.2 и X200.6→TAIL-R.2. Splice в этом документе не создаётся.

### 1.4 Wires

| ID | From (id.handle) | To (id.handle) | Gauge (mm²) | Color | Via | Notes |
|----|------------------|----------------|-------------|-------|-----|-------|
| w-p13 | c-x200.x200-1 | c-tail-l.tl-1 | 0.75 | белый | — | Габарит левый задний |
| w-p14 | c-x200.x200-2 | c-tail-r.tr-1 | 0.75 | голубой | — | Габарит правый задний |
| w-p15 | c-x200.x200-3 | c-tail-l.tl-3 | 0.75 | жёлтый | — | Поворотник левый задний (Flash) |
| w-p16 | c-x200.x200-4 | c-tail-r.tr-3 | 0.75 | зелёный | — | Поворотник правый задний (Flash) |
| w-p17 | c-x200.x200-5 | c-tail-l.tl-2 | 1.0 | фиолетовый | — | Стоп левый |
| w-p17a | c-x200.x200-6 | c-tail-r.tr-2 | 1.0 | фиолетовый | — | Стоп правый |
| w-p18 | c-x200.x200-7 | c-fuel.f-1 | 2.5 | красный | — | Бензонасос (CH3 MICT) |

**GND-провода:** Не включены в жгут. По концепции локальной массы, GND-пины X200 (cavities 8–14) замыкаются на кузов короткими проводами в ответной части разъёма. В TAIL-L/R cavity 4 (GND) тоже подключается к локальной массе. В harness.design GND-пины не подключаются проводами жгута.

### 1.5 Twisted Wires

Нет. Нет дифференциальных сигналов (CAN, LIN) в задней косе.

### 1.6 Bundles

| ID | From | To | Wires in bundle | Length (mm) | Covering |
|----|------|----|-----------------|-------------|----------|
| b-main | c-x200 | bp-trunk | w-p13, w-p14, w-p15, w-p16, w-p17, w-p17a, w-p18 | 2500 | PET braid + 3M Temflex |
| b-fuel | bp-trunk | c-fuel | w-p18 | 600 | 3M Temflex |
| B3 | bp-trunk | bp-tail-split | w-p13, w-p14, w-p15, w-p16, w-p17, w-p17a | 600 | 3M Temflex |
| B5 | bp-tail-split | c-tail-r | w-p14, w-p16, w-p17a | 600 | 3M Temflex |
| B4 | c-tail-l | bp-tail-split | w-p13, w-p15, w-p17 | — | 3M Temflex |

### 1.7 Branch Points

| ID | Layout position | Bundles meeting here | Notes |
|----|----------------|---------------------|-------|
| bp-trunk | center-right | b-main, b-fuel, B3 | Точка разветвления в багажнике |
| bp-tail-split | center | B3, B4, B5 | Разветвление фонарей (левый / правый) |

### 1.8 Mates

Нет. Все коннекторы в этом жгуте — device-side (подключаются к внешним устройствам). Mate с X200-panel будет на центральной панели.

## Verification Checklist

- [ ] X200: 16 cavities (7 power + 7 GND + 2 NC) — соответствует connectorPart.numberOfCavities
- [ ] TAIL-L/R: 4 cavities каждая
- [ ] FUEL: 2 cavities, no shell
- [ ] 7 проводов от X200 до устройств (P13–P18)
- [ ] Все провода подключены к существующим cavity
- [ ] GND-пины не подключены проводами (локальная масса)
- [ ] Splice s-6 НЕ в этом жгуте (на центральной панели)
- [ ] Нет duplicate IDs
- [ ] Все bundles содержат хотя бы один провод
- [ ] Цветовая кодировка соответствует брифу
- [ ] Bundle B4 (к TAIL-L) имеет длину — иначе hasUndefinedSegment на проводах

## Estimated Scope

- Connectors: 4 (X200, TAIL-L, TAIL-R, FUEL)
- Wires: 7
- Splices: 0 (s-6 на центральной панели)
- Bundles: 5 (main + fuel + промежуточный + ветки к TAIL-L/R)
- Branch Points: 2 (bp-trunk, bp-tail-split)
- Parts: 4 connectorParts + 3 contactParts + 3 wireParts + 2 coveringParts ≈ 12
- Operations: ~25–30 (edit_document calls)

## Risks

1. **X200 pin count mismatch** — Центральная панель (HARN-0001) описывает X200 как 8-pin. Задняя коса использует 16-pin. Решение: 16-pin корректен (подтверждено пользователем). При обновлении центральной панели нужно будет согласовать.
2. **GND wiring in harness.design** — GND-пины X200 и TAIL-L/R не подключены проводами жгута (локальная масса). В harness.design GND-пины могут остаться без подключений — это допустимо, но нужно пометить сигналы.
3. **No part numbers** — Конкретные part numbers для разъёмов не определены. Используем generic-описания (Deutsch DT 16-pin, 4-pin combo, 2-pin terminal).
