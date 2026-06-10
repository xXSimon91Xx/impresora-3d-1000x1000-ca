# Problema: Slot MOTOR 3 defectuós a l'Octopus Pro

## Descripció del problema

L'eix Z utilitza **dos motors NEMA 23 independents** per nivelar el llit automàticament amb `Z_TILT_ADJUST`. La configuració inicial assignava:

- Z esquerra → MOTOR 1
- Z dreta → MOTOR 3

El Z dret (MOTOR 3) **no responia**. El motor no es movia, Klipper no mostrava cap error, i el driver semblava funcionar.

## Diagnòstic

Procés d'eliminació:

1. **És el driver?** → Connectem el TMC5160 del MOTOR 3 al slot MOTOR 1. El motor va funcionar. **El driver estava bé.**

2. **És el motor?** → Connectem el motor del Z dret al MOTOR 1. El motor va funcionar. **El motor estava bé.**

3. **És el slot?** → Connectem un driver diferent al MOTOR 3. El motor seguia sense moure's. **El slot MOTOR 3 de la placa estava defectuós.**

> «el driver no és, ja que he posat el driver de Y en X i es movia X, potser és el slot del motor, mira els comentaris de la placa» — conversa del projecte

## Solució

Moure el motor Z dret al **MOTOR 5** (que estava lliure).

### Canvis a printer.cfg

```ini
# ABANS (amb slot defectuós)
[stepper_z1]
step_pin: PG4    # MOTOR 3 — DEFECTUÓS
dir_pin: PC1
enable_pin: !PA0

# DESPRÉS (amb slot funcional)
[stepper_z1]
step_pin: PC13   # MOTOR 5 — FUNCIONAL
dir_pin: !PF0    # ! invertit perquè giri en la mateixa direcció que Z
enable_pin: !PF1
```

> **Nota sobre el `!` a `dir_pin`:** En canviar de slot, el motor girava en sentit contrari al Z esquerre. Afegint `!` (inversió) al `dir_pin` es corregeix sense necessitat de refer el cablejat físic.

## Resultat

Tots dos motors Z funcionen correctament amb `Z_TILT_ADJUST`, que nivella els dos costats del llit de forma independent.

## Lliçó

**Sempre comprovar el slot abans d'assumir que el problema és el driver o el motor.**  
La BTT Octopus Pro pot tenir slots defectuosos de fàbrica — no és un error de l'usuari.

Si un motor no respon:
1. Prova el driver en un altre slot
2. Prova el motor en un altre slot
3. Si tots dos funcionen en altres slots → el slot original és mort
