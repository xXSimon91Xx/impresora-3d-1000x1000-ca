# Cablejat — Eix X

## Components

| Component | Especificació |
|-----------|---------------|
| Motor | NEMA 17 |
| Driver | TMC2209 (UART) |
| Slot placa | MOTOR 0 |
| Endstop | Final de carrera mecànic |
| Transmissió | Corretja GT2, politja 20 dents |

## Configuració a printer.cfg

```ini
[stepper_x]
step_pin: PF13
dir_pin: PF12
enable_pin: !PF14
microsteps: 16
rotation_distance: 40      # politja 20T × pas GT2 2mm = 40mm/volta
endstop_pin: ^PG6          # ^ = pullup activat
position_endstop: 0        # home a la posició 0 (extrem esquerre)
position_max: 1000         # recorregut màxim 1000mm
homing_speed: 50

[tmc2209 stepper_x]
uart_pin: PC4
run_current: 0.600
hold_current: 0.300
stealthchop_threshold: 999999   # StealthChop sempre actiu
```

## Càlcul de rotation_distance

```
rotation_distance = dents_politja × pas_corretja
rotation_distance = 20 × 2mm = 40mm
```

Amb `microsteps: 16` i un motor de 200 passos/volta:
```
Resolució = 40mm / (200 × 16) = 0.0125mm = 12.5 micres per pas
```

## Cablejat del motor

El motor NEMA 17 té 4 cables que formen 2 bobines (A i B):

```
Motor NEMA17 → connector JST a MOTOR 0
  A+ → pin 1
  A- → pin 2
  B+ → pin 3
  B- → pin 4
```

> Si el motor vibra sense moure's, l'ordre A+/A- o B+/B- està invertit. Intercanviar els dos cables d'una bobina.

## Cablejat de l'endstop

```
Endstop X → STOP_0 (PG6)
  Senyal → PG6
  GND    → GND del connector
```

El `^` a `endstop_pin: ^PG6` activa la resistència de pullup interna del STM32.

## Diagrama simplificat

```
MOTOR 0 ─────────── NEMA17 eix X
    step: PF13
    dir:  PF12
    ena:  !PF14
    uart: PC4 (TMC2209)

STOP_0  ─────────── Endstop X (PG6)
```
