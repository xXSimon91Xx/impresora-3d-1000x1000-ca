# Problema: Eix Y — Motor no es movia (cable) + Actualització a motor dual

## Part 1: Motor Y no es movia — era el cable

### Descripció

El motor Y (inicialment un sol motor) no responia als comandaments de moviment. El driver TMC2209 estava correctament configurat i el slot de la placa semblava funcionar.

### Diagnòstic

Vam seguir el mateix procés d'eliminació que amb el slot MOTOR 3:
1. Vam moure el driver Y al slot X → el motor X movia l'eix X → **driver OK**
2. Vam comprovar senyals al slot Y → senyals correctes → **slot OK**

La causa: **el cable del motor Y estava mal connectat** al connector JST de la placa.

> «OK ho he arreglat, era el cable efectivament» — conversa del projecte

### Solució

Tornar a connectar el cable del motor Y assegurant que l'ordre dels fils (A+, A-, B+, B-) és correcte.

### Lliçó

Abans de canviar configuració, **sempre comprovar els cables físicament**:
- Desconnectar i tornar a connectar el connector JST
- Verificar l'ordre dels fils (A+, A-, B+, B-)
- Un fil invertit pot fer que el motor vibri sense moure's o no respongui

---

## Part 2: Actualització a motor Y dual en paral·lel

### Per què un sol motor no era suficient

Amb una cama de **1000mm de profunditat**, l'eix Y necessitava tracció als dos extrems. Un sol motor en un extrem causaria:
- Flexió del perfil d'alumini en l'extrem sense tracció
- Pèrdua de passos en moviments ràpids
- Possible vibració/resonància asimètrica

### Solució: 2 motors NEMA 17 en paral·lel

Tots dos motors es connecten **en paral·lel** al mateix slot del driver (MOTOR 2_1), usant el slot MOTOR 2_2 únicament com a pas físic de corrent (sense configuració pròpia).

```ini
[stepper_y]
step_pin: PF11    # MOTOR 2_1 — un sol driver per a tots dos motors
dir_pin: PG3
enable_pin: !PG5
microsteps: 16
rotation_distance: 40
endstop_pin: ^PG9
position_endstop: 0
position_max: 1000
homing_speed: 50

[tmc2209 stepper_y]
uart_pin: PC6     # UART del slot MOTOR 2_1
run_current: 1.000  # 1A total = ~0.5A per motor en paral·lel
hold_current: 0.500
stealthchop_threshold: 999999
```

> El segon motor (al slot 2_2) es connecta físicament en paral·lel al primer. No té secció a printer.cfg perquè comparteix el driver.

### Per què no usar 2 drivers independents (com a Z)?

A Z usem `Z_TILT_ADJUST` que necessita drivers independents per moure cada motor diferent i corregir la inclinació de la cama.

A Y **no necessitem correcció d'inclinació** — tots dos motors sempre es mouen exactament igual. Per tant, 1 driver en paral·lel és suficient i més senzill.

### Resultat

Amb dos motors en paral·lel:
- Tracció simètrica als dos extrems de l'eix
- Sense pèrdua de passos en moviments ràpids
- Sense vibració asimètrica
