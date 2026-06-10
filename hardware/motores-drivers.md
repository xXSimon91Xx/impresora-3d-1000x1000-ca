# Motors i Drivers — Per què vam triar cada un

> Aquesta secció explica l'elecció dels motors i drivers per a cada eix, amb el raonament tècnic darrere de cada decisió.

---

## Resum ràpid

| Eix | Motor | Driver | Corrent | Protocol |
|-----|-------|--------|---------|----------|
| X | NEMA 17 | TMC2209 | 0.6A run | UART |
| Y (×2 en paral·lel) | NEMA 17 | TMC2209 | 1.0A run | UART |
| Z esquerra | NEMA 23 | TMC5160-T | 2.0A run | SPI |
| Z dreta | NEMA 23 | TMC5160-T | 2.0A run | SPI |
| Extrusor | LDO-36STH20 | TMC2209 | 0.85A run | UART |

---

## TMC2209 vs TMC5160 — Quan usar cada un?

### TMC2209 (UART)

```
Corrent màxim recomanat: ~1.2A continu
Protocol:                UART (1 pin de dades)
StealthChop:             Sí
StallGuard:              Sí (homing sense sensor)
Preu:                    ~3-5€/unitat
```

**Quan usar-lo:** Motors NEMA 17 amb corrents baixos o mitjans (X, Y, extrusor).

**Avantatge clau:** Un únic pin UART per a configuració i telemetria. El driver informa en temps real a Klipper de temperatura, corrent i càrrega.

**Limitació:** No pot gestionar 2A de forma contínua. Si s'usa amb un NEMA 23, el xip s'escalfa i pot fallar.

---

### TMC5160-T Pro (SPI)

```
Corrent màxim:  4.4A de pic / 3A RMS
Protocol:       SPI (4 fils: MISO, MOSI, SCK, CS)
StealthChop:    Sí
Tensió:         8–60V
Color:          Vermell (a la nostra placa)
Preu:           ~8-12€/unitat
```

**Quan usar-lo:** Motors NEMA 23 que necessiten 2A o més.

**Avantatge clau:** Pot gestionar corrents alts sense sobreescalfar-se. Els TMC5160 de la nostra placa funcionen a **2A run / 1A hold** per als NEMA 23 de l'eix Z — els TMC2209 no ho aguantarien.

**Configuració SPI compartida:** Tots els TMC5160 comparteixen els mateixos pins MISO/MOSI/SCK, però cada un té el seu propi `cs_pin` (Chip Select):
```ini
[tmc5160 stepper_z]
cs_pin: PD11    # MOTOR 1
spi_software_miso_pin: PA6
spi_software_mosi_pin: PA7
spi_software_sclk_pin: PA5

[tmc5160 stepper_z1]
cs_pin: PE4     # MOTOR 5
# mateixos pins SPI
```

---

## NEMA 17 vs NEMA 23 — Per què NEMA 23 a Z?

### NEMA 17 (eixos X, Y, extrusor)

```
Mida del marc: 42 × 42 mm
Parell típic:  40–60 N·cm
Corrent:       0.4–1.7A
Pes:           ~250-350g
```

Per a X i Y: el carro només porta el capçal d'impressió (lleuger), de manera que el NEMA 17 és més que suficient.

### NEMA 23 (eix Z)

```
Mida del marc: 57 × 57 mm
Parell típic:  100–200 N·cm
Corrent:       2–4A
Pes:           ~600-800g
```

**Per què NEMA 23 a Z?**  
En aquesta impressora **el llit no es mou** — l'eix Z eleva tot el pòrtic (eix X + carro + capçal extrusor), que pesa diversos quilograms. L'eix Z ha de:
1. Elevar aquest pes contra la gravetat a cada canvi de capa
2. Mantenir el pòrtic estable durant la impressió
3. Fer micro-ajustos independents i precisos per a `Z_TILT_ADJUST`

Un NEMA 17 no tindria prou parell i perdria passos pujant.

---

## Eix Y — Motor dual en paral·lel amb 1 driver

### Per què 2 motors a Y

Amb **1000mm de recorregut en Y**, un motor únic en un extrem generaria un moment de força que desplaçaria lateralment el carro durant el moviment. Amb un motor a cada costat de l'eix, l'empenta és simètrica: el carro es manté centrat i alineat sense desviació lateral ni vibració asimètrica.

### Per què 1 driver per a 2 motors

Els dos motors estan connectats **en paral·lel** al mateix slot (MOTOR 2_1 i MOTOR 2_2 connectats físicament al driver de MOTOR 2_1). Avantatges:
- Estalviem un slot de driver
- Tots dos motors es mouen **exactament igual** (mateix driver = mateixos senyals)
- Cap risc de desincronització

**Inconvenient:** El corrent es reparteix entre els dos motors. Amb `run_current: 1.0A`, cada motor rep 0.5A — suficient per a NEMA 17 a Y.

```ini
[stepper_y]
step_pin: PF11    # MOTOR 2_1 — driver únic
# El segon motor (MOTOR 2_2) es connecta en paral·lel al mateix slot
# No té cap secció separada a printer.cfg
```

### Per què NO usar Z_TILT per a Y?

Podríem haver posat 2 drivers independents per a Y (com fem per a Z amb Z_TILT), però a Y no cal correcció de basculament — la correa ja garanteix que els dos extrems estiguin sincronitzats.

---

## Extrusor — LDO Smart Orbiter v3.0 amb TMC2209

L'extrusor és especial: volem la **màxima resposta** als canvis de velocitat (acceleració/desacceleració ràpida del filament).

Per això el TMC2209 de l'extrusor té **StealthChop desactivat**:

```ini
[tmc2209 extruder]
stealthchop_threshold: 0   # 0 = StealthChop SEMPRE desactivat
```

StealthChop fa el motor més silenciós però menys reactiu. Per a l'extrusor preferim la precisió al silenci.

---

## Configuració de corrents

El corrent correcte per a cada driver es calcula com:

```
run_current = I_nominal_motor × 0.707
```

Per exemple, per al Smart Orbiter v3.0 (corrent nominal del motor: 1A):
```
run_current = 1.0 × 0.707 ≈ 0.707A → posem 0.85A (lleugerament superior per al SO3)
```

Per al NEMA 23 (corrent nominal: ~2.8A), posem 2.0A — limitat per la capacitat de la font d'alimentació i la calor generada.

| Motor | Corrent nominal | run_current | hold_current |
|-------|----------------|-------------|--------------|
| X (NEMA17) | ~1.5A | 0.6A | 0.3A |
| Y (NEMA17 ×2 paral·lel) | ~1.5A | 1.0A (total) | 0.5A |
| Z (NEMA23) | ~2.8A | 2.0A | 1.0A |
| Extrusor (LDO) | ~1.0A | 0.85A | 0.1A |
