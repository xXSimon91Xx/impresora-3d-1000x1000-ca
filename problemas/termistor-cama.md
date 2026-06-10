# Problema: Termistor de la cama calefactada no instal·lat

## Descripció

La cama calefactada de 1000×1000mm **encara no té termistor instal·lat**. Klipper necessita un termistor per controlar la temperatura de la cama, però sense sensor el sistema no pot llegir la temperatura real.

## Solució temporal a printer.cfg

Per evitar que Klipper bloquegi l'arrencada per un sensor de temperatura invàlid, s'usa una configuració de pegat:

```ini
[heater_bed]
heater_pin: PA1
sensor_type: EPCOS 100K B57560G104F  # termistor genèric com a substitut provisional
sensor_pin: PF3
control: watermark
min_temp: -110   # valor molt baix per evitar l'error de Klipper sense sensor
max_temp: 130
```

`min_temp: -110` evita que Klipper llanci un error de "temperatura per sota del mínim" quan el pin de temperatura llegeix valors fora de rang (sense termistor connectat).

> **Atenció:** Aquesta configuració no és segura per a ús real. Sense termistor, Klipper no pot detectar si la cama es sobreescalfa.

## Solució definitiva

Instal·lar un termistor **NTC 100K** (tipus EPCOS B57560G104F o equivalent) a la cama:

1. Enganxar el termistor a la part inferior de la cama calefactada amb cinta kapton
2. Passar el cable fins a l'electrònica
3. Connectar al pin **PF3** (cama T0 a l'Octopus Pro)
4. Canviar `min_temp: -110` per `min_temp: 0` (o el mínim real)

Després recalibrar el PID de la cama:
```
PID_CALIBRATE HEATER=heater_bed TARGET=60
SAVE_CONFIG
```
