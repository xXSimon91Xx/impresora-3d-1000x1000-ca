# Calibració — PID, Z offset, Bed Mesh, Z_TILT

> Tots els passos de calibració necessaris per a una impressió correcta.

---

## 1. PID del hotend

El PID controla el calefactor del hotend per mantenir la temperatura estable.

### Procediment

```gcode
PID_CALIBRATE HEATER=extruder TARGET=220
SAVE_CONFIG
```

Klipper oscil·la la temperatura i calcula els valors òptims K_p, K_i, K_d. Els desa automàticament a `printer.cfg`.

### Els nostres valors calibrats

```ini
#*# [extruder]
#*# control = pid
#*# pid_kp = 22.602
#*# pid_ki = 1.408
#*# pid_kd = 90.690
```

---

## 2. PID del llit (pendent)

Un cop instal·lat el termistor:

```gcode
PID_CALIBRATE HEATER=heater_bed TARGET=60
SAVE_CONFIG
```

---

## 3. Z_TILT_ADJUST — Anivellament Z dual

Iguala els dos motors Z perquè el llit quedi perfectament horitzontal.

### Procediment

```gcode
G28         # Home complet primer
Z_TILT_ADJUST
```

Klipper:
1. Sondeja el punt esquerre del llit
2. Sondeja el punt dret
3. Calcula la diferència d'alçada
4. Ajusta independentment cada motor Z

### Configuració

```ini
[z_tilt]
z_positions:
    -50, 150    # Posició física del fusell esquerre
    350, 150    # Posició física del fusell dret
points:
    30, 150     # Punt de sondeig esquerre
    270, 150    # Punt de sondeig dret
speed: 50
horizontal_move_z: 5
```

---

## 4. Z Offset — Distància broquet-llit

Calibra la distància exacta entre el broquet i el llit quan el CR Touch detecta el "zero".

### Procediment

```gcode
G28
PROBE_CALIBRATE
```

Usa el **mètode del paper**:
1. Col·loca un full sota el broquet
2. Baixa amb `TESTZ Z=-0.05` fins que el paper tingui lleugera resistència
3. Confirma amb `ACCEPT`
4. Desa amb `SAVE_CONFIG`

```gcode
# Comandes d'ajust fi:
TESTZ Z=-0.1    # baixar 0.1mm
TESTZ Z=+0.05   # pujar 0.05mm
TESTZ Z=-0.025  # micro-ajust
```

> El z_offset real es desa a `#*# SAVE_CONFIG`. Varia segons el muntatge físic del CR Touch.

---

## 5. Bed Mesh — Mapa del llit

Mapeja la superfície completa del llit i compensa les irregularitats en temps real.

### Procediment

```gcode
G28
Z_TILT_ADJUST    # sempre abans del bed mesh
BED_MESH_CALIBRATE
SAVE_CONFIG
```

Amb `probe_count: 5, 5`, el CR Touch sondeja **25 punts** en una graella 5×5 entre `mesh_min: 30,30` i `mesh_max: 970,970`.

### Activar a cada impressió

La macro START_PRINT ho fa automàticament:

```ini
[gcode_macro START_PRINT]
gcode:
  G28
  Z_TILT_ADJUST
  BED_MESH_CALIBRATE
  # ... resta de la seqüència
```

---

## 6. Calibració de l'extrusor (rotation_distance)

```gcode
# 1. Temperatura mínima per extruir
M109 S200

# 2. Marcar filament a 100mm de l'entrada de l'extrusor
# 3. Extruir exactament 100mm
G92 E0
G1 E100 F100

# 4. Mesurar quant filament ha entrat realment (amb regle)
# Exemple: han entrat 97.3mm en lloc de 100mm

# 5. Calcular nou rotation_distance:
# nou = actual × (demanat / mesurat)
# 4.637 × (100 / 97.3) = 4.76 → actualitzar a printer.cfg
```

El nostre valor final calibrat: **`rotation_distance: 4.637`**

---

## 7. Calibració d'impressió — Peces de prova

Un cop calibrat el firmware, cal verificar que la impressora produeix peces correctes dimensionalment.

### Cub de calibració 20×20×20mm

Imprimir un cub de 20×20×20mm i mesurar-lo amb calibre:

```
Objectiu: 20.0mm en X, Y i Z
Tolerància acceptable: ±0.2mm
```

Si hi ha desviació:
- **X/Y**: ajustar `rotation_distance` de l'stepper corresponent
- **Z**: ajustar `rotation_distance` del fusell (actualment 8mm/volta)
- **Extrusor (desbordament/buit)**: ajustar `rotation_distance` de l'extrusor

### Benchy (3DBenchy)

Imprimir el model estàndard de referència [3DBenchy](https://www.3dbenchy.com/). Avaluar:
- Ponts sense suport (zona de la cabina)
- Voladissos
- Text en relleu
- Toleràncies en orificis circulars

### Arxius STL de toleràncies

Imprimir peces d'ajust (tolerance test) per mesurar holgures reals entre peces assemblades. Útil abans d'imprimir peces funcionals amb toleràncies precises.

---

## Ordre de calibració recomanat

1. Flashejar firmware
2. Copiar `printer.cfg` base
3. Verificar que tots els motors es mouen correctament
4. `PID_CALIBRATE` hotend
5. `G28` (home complet)
6. `PROBE_CALIBRATE` (z_offset)
7. `Z_TILT_ADJUST`
8. `BED_MESH_CALIBRATE`
9. Calibrar extrusor (rotation_distance)
10. `PID_CALIBRATE` llit (quan s'instal·li el termistor)
