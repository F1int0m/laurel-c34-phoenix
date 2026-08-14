# Build Plan — HARN-0001

**Source brief:** harnesses/HARN-0001-central-panel/README.md
**Generated:** 2026-08-15
**Target harness:** new (documentId will be assigned on creation)
**Strategy:** single-phase — центральная панель как единый модуль (10 коннекторов/терминалов, ~35 проводов, 5 сплайсов)

## Build Order

```text
Parts (connectorParts, contactParts, wireParts)
  → Connectors + Cavities
    → Terminals (BUS+, BUS-)
      → Splices (S1–S5)
        → Wires
          → Bundles + Layout Points
            → Coverings
              → Mates
```

## Open Questions Resolved for Build

| Вопрос | Решение для плана | Примечание |
|--------|-------------------|------------|
| STA_ECU источник | STA от CH5 (до реле) | ЭБУ видит команду запуска даже при неисправном реле — это стандартная практика |
| CH8 Reserve | Не подключаем, MICT.8 = пустая клемма | Добавим cavity в MICT, но провод не тянем |
| Типы разъёмов X100/X200/X300 | Deutsch DT с shell | Указано в брифе как «Deutsch DT (или аналог)» |
| Длины трасс | Используем приблизительные из брифа | Точные потребуют обмера, но для сборки достаточны |

## 1. Parts

### 1.1 connectorParts

| id | partNumber | numberOfCavities | hasShell | Notes |
|----|------------|------------------|----------|-------|
| cp-mict | MICTUNING-P1B | 13 | false | 12 каналов + BATT input; винтовые клеммы |
| cp-x100 | DT-14-P | 14 | true | Передняя коса (panel side) |
| cp-x200 | DT-8-P | 8 | true | Задняя коса (panel side) |
| cp-x300 | DT-8-P | 8 | true | Салонная коса (panel side) |
| cp-relay | BOSCH-ISO-4 | 4 | false | Реле стартера 4-конт. |
| cp-kill | KILL-SWITCH-2 | 2 | false | Kill switch |
| cp-fuse-batt-ecu | FUSE-MINI-2 | 2 | false | Держатель Mini предохранителя (10A) |
| cp-fuse-obd | FUSE-MINI-2 | 2 | false | Держатель Mini предохранителя (5A) |
| cp-fuse-main | FUSE-ANL-2 | 2 | false | Держатель ANL предохранителя (150-200A) |

### 1.2 contactParts

| id | partNumber | Notes |
|----|------------|-------|
| kpt-dt-14 | DT-14-socket | Гнёзда для X100 (14 шт.) |
| kpt-dt-8 | DT-8-socket | Гнёзда для X200, X300 (8+8 шт.) |
| kpt-relay | ISO-relay-socket | Контакты для реле гнезда (4 шт.) |
| kpt-mict | MICT-P1B-socket | Контакты для MICTUNING (13 шт.) |
| kpt-kill | KILL-socket | Контакты kill switch (2 шт.) |
| kpt-fuse-mini | Mini-fuse-socket | Контакты Mini предохранителей (2×2 шт.) |
| kpt-fuse-anl | ANL-fuse-socket | Контакты ANL предохранителя (2 шт.) |

### 1.3 wireParts

| id | gauge (mm2) | color | Notes |
|----|-------------|-------|-------|
| wp-050-red | 50 | красн. | Силовые: АКБ→KILL→FUSE→BUS+, реле→стартер |
| wp-080-red | — | красн. | 80A кабель: BUS+ → MICT BATT (кабель, не провод) |
| wp-025-blu | 2.5 | синий | Fan, Fuel, Heater, GND |
| wp-025-red | 2.5 | красн. | PWR_START силовой вход |
| wp-025-blk | 2.5 | чёрн. | Grounds: GND_ENG, GND_REAR, GND_CABIN |
| wp-015-red | 1.5 | красн. | Headlight L |
| wp-015-blk | 1.5 | чёрн. | Headlight R |
| wp-015-blu | 1.5 | синий | Wiper |
| wp-075-red | 0.75 | красн. | CH1 IGN→ECU, BATT ECU, Stop out |
| wp-075-grn | 0.75 | зелён. | CH5 Start relay coil, IGN→OBD |
| wp-075-wht | 0.75 | белый | CH10 Park L front |
| wp-075-lbl | 0.75 | голуб. | CH10 Park R front |
| wp-075-yel | 0.75 | жёлтый | CH11 L Turn |
| wp-075-grn-turn | 0.75 | зелён. | CH12 R Turn |
| wp-075-org | 0.75 | оранж. | STA signal ECU |
| wp-075-yel-stop | 0.75 | жёлт. | CH6 Stop in |
| wp-075-vio | 0.75 | фиолет. | Stop out → X200.4 |
| wp-075-wht-bl | 0.75 | бел./голуб. | CH10 Park rear (X200.1) |

> **Note:** 50 мм2 и 80A кабель — силовые. В harness.design они моделируются как провода большого сечения. 80A кабель BUS+→MICT — это готовый кабель с клеммами, моделируем как провод.

## 2. Connectors

| id | label | partId | Cavities | Shell | Notes |
|----|-------|--------|----------|-------|-------|
| c-mict | MICT | cp-mict | 13 (1–13) | No | 12 каналов (1–12) + BATT input (13) |
| c-x100 | X100 | cp-x100 | 14 (1–14) | Yes | Передняя коса, panel side |
| c-x200 | X200 | cp-x200 | 8 (1–8) | Yes | Задняя коса, panel side |
| c-x300 | X300 | cp-x300 | 8 (1–8) | Yes | Салонная коса, panel side |
| c-relay | RELAY_START | cp-relay | 4 (1–4) | No | Реле стартера Bosch/ISO |
| c-kill | KILL | cp-kill | 2 (1–2) | No | Kill switch |
| c-fuse-batt-ecu | FUSE_BATT_ECU | cp-fuse-batt-ecu | 2 (1–2) | No | Mini fuse 10A |
| c-fuse-obd | FUSE_OBD | cp-fuse-obd | 2 (1–2) | No | Mini fuse 5A |
| c-fuse-main | FUSE_MAIN | cp-fuse-main | 2 (1–2) | No | ANL fuse 150-200A |

### Cavity Assignments

#### MICT (c-mict)

| Cavity id | Signal | Notes |
|-----------|--------|-------|
| 1 | CH1 IGN | |
| 2 | CH2 Fan | |
| 3 | CH3 Fuel | |
| 4 | CH4 Light | |
| 5 | CH5 Start | |
| 6 | CH6 Stop | |
| 7 | CH7 Heater | |
| 8 | CH8 Reserve | Пустой канал |
| 9 | CH9 Wiper | |
| 10 | CH10 Park | |
| 11 | CH11 L Turn | |
| 12 | CH12 R Turn | |
| 13 | BATT input | Питание PDM от шины+ |

#### RELAY_START (c-relay)

| Cavity id | Signal | Notes |
|-----------|--------|-------|
| 1 | Coil + (от CH5) | Управление |
| 2 | Coil − (GND) | Масса катушки |
| 3 | Contact + (от BUS+, силовой вход) | 50 мм2 |
| 4 | Contact NO (к стартеру, выход) | 50 мм2 → X100 |

#### KILL (c-kill)

| Cavity id | Signal | Notes |
|-----------|--------|-------|
| 1 | IN (от АКБ) | 50 мм2 |
| 2 | OUT (к FUSE_MAIN) | 50 мм2 |

#### FUSE_BATT_ECU (c-fuse-batt-ecu)

| Cavity id | Signal | Notes |
|-----------|--------|-------|
| 1 | IN (от BUS+) | |
| 2 | OUT (к X100.10) | |

#### FUSE_OBD (c-fuse-obd)

| Cavity id | Signal | Notes |
|-----------|--------|-------|
| 1 | IN (от CH1/IGN) | |
| 2 | OUT (к X300.5) | |

#### FUSE_MAIN (c-fuse-main)

| Cavity id | Signal | Notes |
|-----------|--------|-------|
| 1 | IN (от KILL) | 50 мм2 |
| 2 | OUT (к BUS+) | 50 мм2 |

## 3. Terminals

| id | label | type | Notes |
|----|-------|------|-------|
| t-bus-plus | BUS+ | Ring | Главная шина + (медная, винтовые зажимы) |
| t-bus-minus | BUS- | Ring | Общая точка массы (звезда) |

Wire connects with handle `"Terminal"`.

## 4. Splices

| id | Circuits joined | Location | Notes |
|----|----------------|----------|-------|
| s-1 | CH1 → X100.9 (IGN ЭБУ) + X300.5 (OBD) | Панель | IGN-шина |
| s-2 | CH4 → X100.2 + X100.3 | Панель | Ближний L+R |
| s-3 | CH10 → X100.5 + X100.6 + X200.1 | Панель | Габариты перед L + перед R + задние |
| s-4 | CH11 → X100.7 + X200.2 | Панель | Поворотник левый перед + зад |
| s-5 | CH12 → X100.8 + X200.3 | Панель | Поворотник правый перед + зад |

## 5. Wires

### 5.1 Питание от MICTUNING

| id | From (id.handle) | To (id.handle) | Gauge (mm2) | Color | Via | Notes |
|----|------------------|----------------|-------------|-------|-----|-------|
| w-ch1-ign | c-mict.1 | s-1.Splice | 0.75 | красн. | — | CH1 → сплайн IGN-шины |
| w-ign-ecu | s-1.Splice | c-x100.9 | 0.75 | красн. | — | IGN → ЭБУ IGSW/E1 |
| w-ign-obd | s-1.Splice | c-fuse-obd.1 | 0.75 | зелён. | — | IGN → предохранитель OBD |
| w-fuse-obd-out | c-fuse-obd.2 | c-x300.5 | 0.75 | зелён. | — | OBD+ после предохранителя |
| w-ch2-fan | c-mict.2 | c-x100.1 | 2.5 | синий | — | Вентилятор |
| w-ch3-fuel | c-mict.3 | c-x200.5 | 2.5 | красн. | — | Бензонасос |
| w-ch4-l | c-mict.4 | s-2.Splice | 1.5 | красн. | — | Ближний свет (вход в сплайн) |
| w-head-l | s-2.Splice | c-x100.2 | 1.5 | красн. | — | Ближний L |
| w-head-r | s-2.Splice | c-x100.3 | 1.5 | чёрн. | — | Ближний R |
| w-ch5-start | c-mict.5 | c-relay.1 | 0.75 | зелён. | — | Катушка реле + |
| w-sta-ecu | c-mict.5 | c-x100.11 | 0.75 | оранж. | — | STA на ЭБУ (от CH5 до реле) |
| w-ch6-stop | c-mict.6 | c-x300.3 | 0.75 | жёлт. | — | Стоп-сигнал (вход выкл.) |
| w-stop-out | c-x300.4 | c-x200.4 | 0.75 | красн. | — | Стоп-сигнал (выход выкл. → зад) |
| w-ch7-heater | c-mict.7 | c-x300.1 | 2.5 | красн. | — | Печка |
| w-ch9-wiper | c-mict.9 | c-x300.2 | 1.5 | синий | — | Дворники |
| w-ch10-lf | c-mict.10 | s-3.Splice | 0.75 | белый | — | Габарит (вход в сплайн) |
| w-park-lf | s-3.Splice | c-x100.5 | 0.75 | белый | — | Передний левый габарит |
| w-park-rf | s-3.Splice | c-x100.6 | 0.75 | голуб. | — | Передний правый габарит |
| w-park-rear | s-3.Splice | c-x200.1 | 0.75 | бел./голуб. | — | Задние габариты |
| w-ch11-lt | c-mict.11 | s-4.Splice | 0.75 | жёлтый | — | Л. поворотник (вход) |
| w-turn-lf | s-4.Splice | c-x100.7 | 0.75 | жёлтый | — | Передний левый поворотник |
| w-turn-lr | s-4.Splice | c-x200.2 | 0.75 | жёлтый | — | Задний левый поворотник |
| w-ch12-rt | c-mict.12 | s-5.Splice | 0.75 | зелён. | — | П. поворотник (вход) |
| w-turn-rf | s-5.Splice | c-x100.8 | 0.75 | зелён. | — | Передний правый поворотник |
| w-turn-rr | s-5.Splice | c-x200.3 | 0.75 | зелён. | — | Задний правый поворотник |

### 5.2 Питание от шины+ (минуя MICTUNING)

| id | From (id.handle) | To (id.handle) | Gauge (mm2) | Color | Via | Notes |
|----|------------------|----------------|-------------|-------|-----|-------|
| w-pwr-mict | t-bus-plus.Terminal | c-mict.13 | 80A кабель | красн. | — | Питание PDM от шины+ |
| w-pwr-start-in | t-bus-plus.Terminal | c-relay.3 | 50 | красн. | — | Силовой вход реле |
| w-start-out | c-relay.4 | c-x100.4 | 50 | красн. | — | Пусковой кабель → стартер |
| w-batt-ecu-in | t-bus-plus.Terminal | c-fuse-batt-ecu.1 | 0.75 | красн. | — | BATT ECU до предохранителя |
| w-batt-ecu-out | c-fuse-batt-ecu.2 | c-x100.10 | 0.75 | красн. | — | BATT ECU после предохранителя |
| w-relay-gnd | c-relay.2 | t-bus-minus.Terminal | 0.75 | чёрн. | — | Масса катушки реле |

### 5.3 Силовой ввод (АКБ → шина+)

| id | From (id.handle) | To (id.handle) | Gauge (mm2) | Color | Via | Notes |
|----|------------------|----------------|-------------|-------|-----|-------|
| w-bat-in | t-bus-plus.Terminal | c-kill.1 | 50 | красн. | — | АКБ+ → Kill switch (внеш. АКБ) |
| w-kill-out | c-kill.2 | c-fuse-main.1 | 50 | красн. | — | Kill → FUSE_MAIN |
| w-fuse-out | c-fuse-main.2 | t-bus-plus.Terminal | 50 | красн. | — | FUSE_MAIN → шина+ |

> **Важно:** w-bat-in — это внешнее подключение от АКБ в багажнике. В харнесе моделируется как провод от terminal BUS+ к KILL.1. Физически АКБ находится за пределами панели, но для схемы провод подключается к BUS+, а затем от BUS+ через KILL и FUSE_MAIN возвращается на BUS+. Реальная топология: АКБ → KILL → FUSE → BUS+. В схеме BUS+ — центральный узел.

**Схема силового ввода (исправленная):**

```
Внеш. АКБ+ ──── c-kill.1 ── c-kill.2 ── c-fuse-main.1 ── c-fuse-main.2 ── t-bus-plus
```

Провод w-bat-in не нужен как отдельный — вместо него:

| id | From (id.handle) | To (id.handle) | Gauge (mm2) | Color | Notes |
|----|------------------|----------------|-------------|-------|-------|
| w-kill-in | (внеш. АКБ) | c-kill.1 | 50 | красн. | Внешний провод, не в харнесе |
| w-kill-fuse | c-kill.2 | c-fuse-main.1 | 50 | красн. | Kill → FUSE_MAIN |
| w-fuse-bus | c-fuse-main.2 | t-bus-plus.Terminal | 50 | красн. | FUSE_MAIN → шина+ |

> w-kill-in — вне харнеса (от АКБ в багажнике). Моделируем как: от отдельного terminal или просто оставляем c-kill.1 как входной контакт для внешнего провода.

### 5.4 Массовые цепи

| id | From (id.handle) | To (id.handle) | Gauge (mm2) | Color | Notes |
|----|------------------|----------------|-------------|-------|-------|
| w-gnd-eng | c-x100.12 | t-bus-minus.Terminal | 2.5 | чёрн. | Масса двигателя → звезда |
| w-gnd-rear | c-x200.6 | t-bus-minus.Terminal | 2.5 | чёрн. | Масса багажника → звезда |
| w-gnd-cabin | c-x300.6 | t-bus-minus.Terminal | 2.5 | чёрн. | Масса салона → звезда |

## 6. Twisted Wires

Нет. В данной конфигурации нет CAN-шины или других дифференциальных пар.

## 7. Bundles

| id | Source | Target | Wires | Approx. Length (mm) | Covering | Notes |
|----|--------|--------|-------|---------------------|----------|-------|
| b-mict-x100 | c-mict | c-x100 | w-ch1-ign, w-ign-ecu, w-ch2-fan, w-ch4-l, w-head-l, w-head-r, w-ch5-start, w-sta-ecu, w-ch10-lf, w-park-lf, w-park-rf, w-ch11-lt, w-turn-lf, w-ch12-rt, w-turn-rf, w-batt-ecu-out, w-start-out, w-gnd-eng | 300 | Tape 3M | Основной жгут → передняя коса |
| b-mict-x200 | c-mict | c-x200 | w-ch3-fuel, w-park-rear, w-turn-lr, w-turn-rr, w-stop-out, w-gnd-rear | 200 | Tape 3M | Жгут → задняя коса |
| b-mict-x300 | c-mict | c-x300 | w-ch6-stop, w-ch7-heater, w-ch9-wiper, w-ign-obd, w-fuse-obd-out, w-gnd-cabin | 200 | Tape 3M | Жгут → салонная коса |
| b-power-in | c-kill | t-bus-plus | w-kill-fuse, w-fuse-bus, w-pwr-mict, w-pwr-start-in, w-batt-ecu-in, w-relay-gnd | 300 | PET braid | Силовой ввод |
| b-bus-relay | t-bus-plus | c-relay | w-pwr-start-in, w-relay-gnd | 200 | PET braid | BUS+ → реле стартера |

> **Примечание по bundles:** Splices (S1–S5) находятся внутри b-mict-x100 и b-mict-x200, т.к. разветвления происходят по пути к инлайн-разъёмам. Точная топология bundles потребует уточнения при layout — здесь указаны логические группы.

## 8. Mates

| Connector A | Connector B | Type | Notes |
|-------------|-------------|------|-------|
| c-x100 | (X100-panel) | Inline | Mate будет добавлен когда передняя коса будет построена |
| c-x200 | (X200-panel) | Inline | Mate будет добавлен когда задняя коса будет построена |
| c-x300 | (X300-panel) | Inline | Mate будет добавлен когда салонная коса будет построена |

> Mates для инлайн-разъёмов создаются в соответствующих модулях (front/rear/cabin). В данном харнесе — только panel side.

## 9. Schematic Positioning

Все компоненты в schematic view. Ориентировочная раскладка (grid = 30 units):

```
                    c-kill(0,0)  c-fuse-main(90,0)
                         |
                    t-bus-plus(0,90)
                    /    |    \
            c-mict(−300,180)  c-relay(0,180)  c-fuse-batt-ecu(90,180)
              /    |    \
     c-x100(−450,360) c-x200(−150,360) c-x300(150,360)

    t-bus-minus(−300,360) — масса (звезда, внизу слева)

    s-1..s-5 — сплайсы между c-mict и c-x100/c-x200
```

> Точные координаты будут скорректированы при сборке. Главное — логическая группировка: силовой ввод сверху, MICT в центре, инлайн-разъёмы снизу.

## 10. Cross-Module Connections

| Module A | Connector | Module B | Connector | Shared circuits |
|----------|-----------|----------|-----------|----------------|
| Central Panel | c-x100 | Front Harness | X100-panel | CH2, CH4, CH5, CH10, CH11, CH12, IGN ЭБУ, BATT ЭБУ, STA, GND, PWR_START |
| Central Panel | c-x200 | Rear Harness | X200-panel | CH3, CH6, CH10, CH11, CH12, GND |
| Central Panel | c-x300 | Cabin Harness | X300-panel | CH6, CH7, CH9, CH1 (OBD), GND |

## Verification Checklist

- [ ] c-mict: 13 cavities, no shell — matches cp-mict (13, hasShell=false)
- [ ] c-x100: 14 cavities, shell — matches cp-x100 (14, hasShell=true)
- [ ] c-x200: 8 cavities, shell — matches cp-x200 (8, hasShell=true)
- [ ] c-x300: 8 cavities, shell — matches cp-x300 (8, hasShell=true)
- [ ] c-relay: 4 cavities, no shell — matches cp-relay (4, hasShell=false)
- [ ] c-kill: 2 cavities, no shell — matches cp-kill (2, hasShell=false)
- [ ] c-fuse-batt-ecu: 2 cavities — matches cp-fuse-batt-ecu (2, hasShell=false)
- [ ] c-fuse-obd: 2 cavities — matches cp-fuse-obd (2, hasShell=false)
- [ ] c-fuse-main: 2 cavities — matches cp-fuse-main (2, hasShell=false)
- [ ] All wires connect valid endpoints (connector.cavity / Terminal / Splice)
- [ ] All splices join ≥2 wires (S1=3, S2=3, S3=4, S4=3, S5=3)
- [ ] No duplicate IDs across document
- [ ] BUS+ and BUS- modeled as Ring terminals
- [ ] Kill switch chain: external АКБ → c-kill.1 → c-kill.2 → c-fuse-main.1 → c-fuse-main.2 → t-bus-plus
- [ ] CH8 Reserve: cavity 8 exists in MICT but no wire connected
- [ ] Cavity 13,14 in X100 are NC (no wires)
- [ ] Cavity 7,8 in X200 are NC
- [ ] Cavity 7,8 in X300 are NC

## Estimated Scope

| Element | Count |
|---------|-------|
| connectorParts | 9 |
| contactParts | 7 |
| wireParts | 18 |
| Connectors | 9 |
| Terminals | 2 |
| Splices | 5 |
| Wires | ~30 |
| Bundles | ~5 |
| Mates | 0 (в этом модуле) |
| Operations (edit_document) | ~8–10 (parts, connectors, terminals, splices, wires, bundles) |

## Risks

| Risk | Mitigation |
|------|------------|
| 50 мм2 провода нестандартны для wire parts | Использовать максимальное сечение в gaugeUnit=mm2; если невозможно — моделировать как cable или отметить как не-BOM |
| STA_ECU: от CH5 или от реле | Решили: от CH5 (до реле). Если нужен от выхода реле — потребуется изменить w-sta-ecu |
| 80A кабель BUS+→MICT — готовый кабель с клеммами | Моделируем как провод; BOM считает длину, но физически это сборная единица |
| CH6 Stop circuit проходит через X300 (выключатель) и обратно в X200 | Корректно: MICT.6 → X300.3 → выключатель → X300.4 → X200.4. Это «сквозная» цепь через салон |
| X100 pin 4 конфликт: CH5 Start (0.75) и PWR_START (50 мм2) | X100.4 = CH5 Start (0.75, слаботочное упр. катушкой). Стартер получает 50 мм2 через реле напрямую — X100.4 это слаботочный сигнал, а не 50 мм2. Исправлено: w-start-out → c-x100.4 не нужен, провод 50 мм2 идёт от реле напрямую к стартеру (вне panel). Убрать w-start-out из таблицы проводов |

### Исправление: X100.4 и стартер

X100.4 = CH5 Start (0.75 мм2, управляющий сигнал на катушку реле). Силовой провод 50 мм2 от реле к стартеру **не проходит** через X100 — он идёт напрямую отдельным кабелем.

Соответственно:
- **Убрать** w-start-out из таблицы 5.2
- c-relay.4 → внешний провод к стартеру (не через X100)
- В X100 cavity 4 — только слаботочный сигнал CH5

---

**Next step:** `/harness-build harnesses/HARN-0001-central-panel/`
