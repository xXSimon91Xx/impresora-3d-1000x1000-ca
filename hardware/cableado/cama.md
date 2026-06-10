# Cablejat — Llit calefactat

## Estat actual

> ⚠️ **El llit calefactat NO està instal·lat encara.**  
> No existeix un llit de 1000×1000mm estàndard — la solució prevista és muntar **4 llits calefactats de 500×500mm** en paral·lel. La configuració actual és un pedaç temporal per poder arrencar Klipper sense llit real.

---

## Components

| Component | Estat | Pin |
|-----------|-------|-----|
| Calefactor de llit | ❌ Pendent d'instal·lar | PA1 (HE_BED) |
| Termistor de temperatura | ❌ Pendent d'instal·lar | PF3 (T1) |

---

## Configuració actual (pedaç temporal)

```ini
[heater_bed]
heater_pin: PA1
sensor_type: EPCOS 100K B57560G104F   # placeholder — no hi ha sensor real
sensor_pin: PF3
control: watermark
min_temp: -110    # molt baix perquè Klipper no falli sense sensor
max_temp: 130
```

`min_temp: -110` evita que Klipper bloquegi l'inici quan el pin PF3 llegeix un valor fora de rang (perquè no hi ha termistor connectat).

---

## Instal·lació pendent del termistor

Per completar el llit calefactat:

1. **Comprar:** 4 llits calefactats estàndard de **500×500mm** i muntar-los cobrint l'àrea 1000×1000mm
2. **Comprar:** Termistor NTC 100K tipus EPCOS B57560G104F (o genèric 100K B3950) — un per zona o un central
3. **Muntar:** Enganxar el termistor a la part inferior de la placa calefactora amb cinta de kapton
4. **Connectar:** Al pin PF3 (connector T1 de l'Octopus Pro)
5. **Actualitzar config:**

```ini
[heater_bed]
heater_pin: PA1
sensor_type: EPCOS 100K B57560G104F
sensor_pin: PF3
control: pid              # canviar de watermark a pid
min_temp: 0               # restaurar a valor normal
max_temp: 130
```

6. **Calibrar PID del llit:**
```
PID_CALIBRATE HEATER=heater_bed TARGET=60
SAVE_CONFIG
```

---

## Cablejat del calefactor

El llit d'1m² necessita un calefactor d'alta potència (típicament 1000-2000W). Els cables van directament al connector BED-OUT de l'Octopus Pro, que alimenta el MOSFET que controla el llit.

```
Llit calefactat:
  Cable 1 (positiu) → BED-OUT +
  Cable 2 (negatiu) → BED-OUT -

Alimentació BED-POWER separada:
  BED-POWER + → font 24V (rail dedicat)
  BED-POWER - → GND font
```

> L'Octopus Pro té entrades d'alimentació **separades** per al llit (BED-POWER) i per a la resta de la placa (POWER). És important mantenir-les separades per evitar soroll als senyals dels motors.
