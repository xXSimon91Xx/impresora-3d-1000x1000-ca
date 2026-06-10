# Cablejat — Eix Z (Dual independent)

> L'eix Z és el més complex del projecte: dos motors NEMA 23 independents amb drivers TMC5160 d'alta potència, configurats per al `Z_TILT_ADJUST` de Klipper.

---

## Components

| Component | Especificació |
|-----------|---------------|
| Motors | 2× NEMA 23 |
| Drivers | 2× TMC5160-T Pro (SPI) |
| Slot Z esquerra | MOTOR 1 |
| Slot Z dreta | MOTOR 5 (MOTOR 3 estava defectuós) |
| Transmissió | Fusell mètric 12, pas 2mm, 4 entrades |
| Endstop Z | CR Touch (virtual) + endstop mecànic de seguretat al tope |

---

## Configuració a printer.cfg

```ini
# Z ESQUERRA — Motor principal
[stepper_z]
step_pin: PG0
dir_pin: PG1
enable_pin: !PF15
microsteps: 16
rotation_distance: 8       # pas 2mm × 4 entrades = 8mm/volta
endstop_pin: probe:z_virtual_endstop  # CR Touch com a endstop
position_min: -2           # permet baixar 2mm per sota 0 per calibrar offset
position_max: 1000
homing_speed: 10

[tmc5160 stepper_z]
cs_pin: PD11               # Chip Select SPI — MOTOR 1
spi_software_miso_pin: PA6
spi_software_mosi_pin: PA7
spi_software_sclk_pin: PA5
run_current: 2.000         # NEMA23 necessita 2A
hold_current: 1.000
stealthchop_threshold: 999999
driver_TOFF: 3
driver_HSTRT: 4
driver_HEND: 3

# Z DRETA — Motor esclau
[stepper_z1]
step_pin: PC13             # MOTOR 5 (MOTOR 3 estava defectuós)
dir_pin: !PF0              # ! invertit — gira en la mateixa direcció que Z
enable_pin: !PF1
microsteps: 16
rotation_distance: 8

[tmc5160 stepper_z1]
cs_pin: PE4                # Chip Select SPI — MOTOR 5
spi_software_miso_pin: PA6
spi_software_mosi_pin: PA7
spi_software_sclk_pin: PA5
run_current: 2.000
hold_current: 1.000
stealthchop_threshold: 999999
driver_TOFF: 3
driver_HSTRT: 4
driver_HEND: 3
```

---

## Per què `dir_pin: !PF0` al Z dret

Els dos motors Z estan muntats a costats oposats de la impressora. Si tots dos giressin en la mateixa direcció absoluta, un baixaria el llit i l'altre el pujaria — el llit es torceria.

Afegint `!` (inversió) al `dir_pin` del Z dret, un motor gira en sentit horari i l'altre en antihorari — però **tots dos eleven o baixen el llit en la mateixa direcció**.

Això es va descobrir durant les proves:
> «Bé funciona bb, vaig a girar el signe d'exclamació perquè un gira al revés bb» — conversa

---

## SPI compartit entre els dos TMC5160

Els dos drivers TMC5160 comparteixen els pins de bus SPI (MISO, MOSI, SCLK) però cada un té el seu propi `cs_pin`:

```
Bus SPI (compartit):
  PA6 ──── MISO  ──── TMC5160 Z esq ──── TMC5160 Z dret
  PA7 ──── MOSI
  PA5 ──── SCLK

Chip Select (individual):
  PD11 ──── CS del TMC5160 Z esq (MOTOR 1)
  PE4  ──── CS del TMC5160 Z dret (MOTOR 5)
```

El Chip Select determina quin driver està actiu en cada moment — els altres ignoren les dades del bus.

---

## Z_TILT_ADJUST — Per a què serveix

Amb dos motors Z independents, Klipper pot **corregir automàticament** si el llit no està perfectament nivelat de costat a costat.

```ini
[z_tilt]
z_positions:
    -50, 150    # posició física del fusell esquerre (X, Y)
    350, 150    # posició física del fusell dret
points:
    30, 150     # on sondeja el CR Touch per al costat esquerre
    270, 150    # on sondeja per al costat dret
speed: 50
horizontal_move_z: 5
```

En executar `Z_TILT_ADJUST`, Klipper:
1. Sondeja el punt esquerre amb el CR Touch
2. Sondeja el punt dret
3. Calcula la diferència
4. Mou independentment cada motor per igualar les alçades

---

## Endstop de seguretat Z màxim

```ini
[gcode_button endstop_z_max]
pin: ~PF7    # T3 — connector de temperatura reconvertit en entrada digital (pulldown)
press_gcode:
  M118 EMERGÈNCIA - Endstop Z màxim activat
  M112         # Parada d'emergència
```

Instal·lat al tope físic superior de les guies Z. Si el programari falla i l'eix puja més del degut, el final de carrera mecànic atura tot amb `M112`.

---

## Càlcul de rotation_distance

```
Fusell M12, 4 entrades, pas 2mm:
rotation_distance = 2mm × 4 = 8mm/volta

Amb microsteps: 16 i 200 passos/volta:
Resolució = 8mm / (200 × 16) = 0.0025mm = 2.5 micres
```
