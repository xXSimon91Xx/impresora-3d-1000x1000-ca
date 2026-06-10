# Problema: Cargol trapezial mètric 12 de 1200mm — difícil de trobar

## Descripció

Per a l'eix Z de 1000mm necessitàvem cargols de, com a mínim, **1200mm de longitud** (amb marge). L'especificació:

- **Tipus:** Cargol trapezial / roscador de moviment lineal
- **Diàmetre:** 12mm (mètric 12)
- **Pas:** 2mm
- **Entrades:** 4 (pas efectiu = 2 × 4 = **8mm per revolució**)
- **Longitud mínima:** 1200mm

> «Tenim un problema per trobar la vareta trapezoïdal, quines solucions ens doneu per aconseguir una vareta de 1000mm» — conversa del projecte

## Per què aquest cargol específic?

### Relació amb rotation_distance

A Klipper, el paràmetre `rotation_distance` per a Z es calcula com:

```
rotation_distance = pas × entrades = 2mm × 4 = 8mm/rev
```

Un cargol T8 (8mm de diàmetre, 2mm de pas, 4 entrades) també donaria 8mm/rev, però amb 8mm de diàmetre es doblegaria a 1m de longitud.

Amb **mètric 12** i 1.2m de longitud, el cargol té rigidesa suficient per no desviar-se.

### Per què 4 entrades i no 1

Un cargol d'1 entrada amb 2mm de pas donaria `rotation_distance = 2`. Això significa:
- Molta resolució (bé)
- Velocitat Z molt lenta (malament — recórrer 1m trigaria molt de temps)

Amb 4 entrades: `rotation_distance = 8`. Velocitat Z màxima de 5mm/s = 5mm/s × 60s = **300mm/min** — manejable.

## On trobar-lo

Proveïdors que solen tenir cargols T12 4 entrades en 1200mm:
- Proveïdors CNC a Espanya (Servocity, Ooznest)
- AliExpress (enviament lent però bon estoc)
- Proveïdors d'automatització industrial

> Assegureu-vos d'especificar: **D=12mm, pas=2mm, 4 entrades (4-start), longitud=1200mm**

## Configuració resultant

```ini
[stepper_z]
rotation_distance: 8   # 2mm de pas × 4 entrades = 8mm/rev
microsteps: 16
```

Amb `microsteps: 16`:
```
Resolució = 8mm / (200 passos × 16) = 0.0025mm = 2.5 microns per pas
```
