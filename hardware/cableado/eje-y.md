# Cablejat — Eix Y (Motor dual en paral·lel)

## Components

| Component | Especificació |
|-----------|---------------|
| Motors | 2× NEMA 17 (en paral·lel) |
| Driver | 1× TMC2209 (UART) — compartit entre 2 motors |
| Slot placa | MOTOR 2_1 (driver) + MOTOR 2_2 (sense driver, paral·lel físic) |
| Endstop | Final de carrera mecànic |
| Transmissió | Corretja GT2, politja 20 dents |

---

## Configuració a printer.cfg

```ini
[stepper_y]
step_pin: PF11             # MOTOR 2_1 — driver únic
dir_pin: PG3
enable_pin: !PG5
microsteps: 16
rotation_distance: 40      # politja 20T × pas GT2 2mm = 40mm/volta
endstop_pin: ^PG9          # ^ = pullup
position_endstop: 0
position_max: 1000
homing_speed: 50

[tmc2209 stepper_y]
uart_pin: PC6              # UART del slot MOTOR 2_1
run_current: 1.000         # 1A total ÷ 2 motors en paral·lel = 0.5A cada un
hold_current: 0.500
stealthchop_threshold: 999999
```

> El segon motor (slot 2_2) es connecta físicament en paral·lel al primer. **No té cap secció pròpia a printer.cfg** — un sol driver els controla a tots dos.

---

## Diagrama de cablejat en paral·lel

```
Driver TMC2209 (MOTOR 2_1)
    └── Motor Y esquerre (MOTOR 2_1)
         ├── A+ ──── A+ motor esquerre
         ├── A- ──── A- motor esquerre
         ├── B+ ──── B+ motor esquerre
         └── B- ──── B- motor esquerre

    └── Motor Y dret (MOTOR 2_2 físicament, paral·lel elèctric)
         ├── A+ ──── A+ motor dret
         ├── A- ──── A- motor dret  
         ├── B+ ──── B+ motor dret
         └── B- ──── B- motor dret
```

Els dos connectors de sortida de l'Octopus Pro per a MOTOR 2_1 i MOTOR 2_2 estan elèctricament connectats — tots dos porten els mateixos senyals step/dir/enable del mateix driver.

---

## Història: d'1 motor a 2 en paral·lel

**Configuració inicial:** 1 sol motor Y a MOTOR 2_1.

**Problema:** Amb 1000mm de recorregut, un sol motor en un extrem causaria:
- Asimetria de forces → l'eix es torça lleugerament
- Pèrdua de passos en acceleracions altes

**Solució:** Afegir un segon motor NEMA 17 en paral·lel a l'altre extrem de l'eix.

Vegeu: [Problema — Eix Y dual](../../problemas/eje-y-dual.md)

---

## Endstop Y

```
Endstop Y → STOP_2 (PG9)
  Senyal → PG9
  GND    → GND
```

L'endstop s'activa quan el carro Y arriba a l'extrem posterior (posició 0). El home mou el carro cap enrere fins activar l'endstop, després recula a position_endstop.
