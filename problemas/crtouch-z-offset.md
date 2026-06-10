# Problema: CR Touch — z_offset obligatori a l'arrencada

## Descripció del problema

Després d'afegir la secció `[bltouch]` a `printer.cfg`, **Klipper no arrencava**:

```
Option 'z_offset' in section 'bltouch' must be specified
```

## Causa

Klipper requereix que el camp `z_offset` estigui **sempre present** a la secció `[bltouch]`, encara que no s'hagi calibrat. Si el camp no existeix, el servidor Klipper no pot carregar la configuració.

## Solució

Afegir `z_offset: 0` temporalment perquè Klipper pugui arrencar:

```ini
[bltouch]
sensor_pin: ^PB7
control_pin: PB6
pin_up_touch_mode_reports_triggered: False
x_offset: 0
y_offset: 0
z_offset: 0        # TEMPORAL — calibrar després amb PROBE_CALIBRATE
speed: 5.0
lift_speed: 10.0
samples: 2
sample_retract_dist: 5.0
```

Després calibrar el offset real amb:

```
PROBE_CALIBRATE
```

Klipper baixa el CR Touch fins que toca la cama i permet ajustar l'offset amb `TESTZ Z=-0.1` (baixar) o `TESTZ Z=+0.1` (pujar). Quan es finalitza, `SAVE_CONFIG` desa el valor real al bloc `#*# SAVE_CONFIG` al final del fitxer.

## Valor calibrat a la nostra màquina

`SAVE_CONFIG` desa automàticament el offset real. El valor varia depenent del muntatge físic del CR Touch respecte al broquet.

## Lliçó

- Klipper valida la configuració completa a l'arrencada — un camp que falta impedeix l'arrencada
- El `z_offset` a `[bltouch]` és un **marcador de posició** fins al calibratge — sempre posar-lo a `0`
- El valor real es desa a `#*# SAVE_CONFIG` — **no editar aquest bloc manualment**
