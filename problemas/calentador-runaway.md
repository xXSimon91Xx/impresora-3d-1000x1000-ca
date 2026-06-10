# Problema: El calentador pujava sol a 220°C

## Descripció del problema

En encendre la impressora, el hotend va començar a escalfar-se **sol** sense cap comandament. La temperatura va pujar fins a 220°C de manera autònoma.

> «No, continua escalfant, ha arribat a 220 graus sense fer res» — conversa del projecte

## Causa

El pin del calentador (`heater_pin`) estava configurat en un pin incorrecte. El HE0 (element calefactor 0) de l'Octopus Pro no estava cablejat correctament:

> «Ei, encara tinc el mateix problema... B:-99.4 /0.0 T0:74.9 /0.0» — conversa  
> «No sé per què però HE0 no funciona» — conversa

El problema era que el calentador físic estava connectat a un pin diferent del configurat a `printer.cfg`. Quan el pin configurat no coincideix amb el real, pot romandre en estat actiu per defecte.

## Solució

Verificar el cablejat físic del calentador:

1. Apagar la impressora immediatament
2. Comprovar a quin connector HE (0, 1, 2 o 3) està connectat el calentador del hotend
3. Verificar que `heater_pin` a printer.cfg coincideix:

```ini
[extruder]
heater_pin: PA3   # PA3 = HE0 a l'Octopus Pro V1.1
```

Mapa de pins HE de l'Octopus Pro V1.1:
- HE0 → PA3
- HE1 → PA4  
- HE2 → PA5  (verificar amb el manual de la placa)
- HE3 → (verificar)

4. Si el calentador continua pujant després de confirmar el pin, comprovar si hi ha un curtcircuit al termistor o al MOSFET de la placa

## Resultat

Amb el pin correcte (`PA3`) i el cablejat verificat, el hotend únicament s'escalfa quan es comanda explícitament amb `M104` o `M109`.

## Lliçó

**Sempre verificar físicament a quin pin està connectat cada component.** La placa té múltiples slots similars (HE0, HE1, HE2, HE3) i és fàcil connectar al slot equivocat. Una discordança entre el cable físic i la configuració pot causar comportaments perillosos.
