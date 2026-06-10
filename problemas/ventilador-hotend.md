# Problema: El ventilador del hotend no s'apagava mai

## Descripció del problema

El ventilador del dissipador de calor de l'extrusor LDO Smart Orbiter v3.0 **no s'apagava mai**, independentment de la temperatura del hotend.

El ventilador havia de funcionar únicament quan el hotend superés una temperatura de seguretat (normalment 50°C). En canvi, estava encès de manera constant fins i tot amb la impressora freda.

## Causa

El pin del ventilador estava configurat com a `[fan]` (ventilador de capa) en lloc de `[heater_fan]` (ventilador del dissipador).

En Klipper:
- `[fan]` = ventilador de capa — el controla el lliscador del gcode (M106/M107)
- `[heater_fan]` = ventilador del dissipador — s'activa automàticament quan el hotend supera un llindar de temperatura

El pin que es va usar inicialment (`PG12`) tampoc era vàlid per a la placa Octopus Pro V1.1.

## Solució

Canviar la configuració al pin correcte (`PD12`, FAN2) i usar la secció `[heater_fan]`:

```ini
[heater_fan hotend_fan]
pin: PD12         # FAN2 a l'Octopus Pro V1.1
max_power: 1.0
heater: extruder
heater_temp: 50.0 # s'activa quan el hotend supera 50°C
```

## Resultat

Amb la configuració correcta:
- El ventilador del dissipador s'activa automàticament quan el hotend supera 50°C
- S'apaga sol quan la temperatura baixa per sota del llindar
- Protegeix el hotend d'una fusió accidental del PTFE quan la impressora es refreda

## Lliçó

- `[fan]` = ventilador de capa (controlat per M106/M107 al gcode)
- `[heater_fan]` = ventilador del dissipador (controlat automàticament per temperatura)
- Sempre verificar que el pin configurat existeix i és vàlid per a la placa
