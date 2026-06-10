# Problemes trobats i solucions

> Aquesta secció documenta tots els problemes reals que vam trobar durant la construcció i com els vam resoldre. Ordenada cronològicament.

---

## Índex

| # | Problema | Impacte | Estat |
|---|----------|---------|-------|
| 1 | [Slot MOTOR 3 defectuós](motor-z-slot-defectuoso.md) | Z dret no funcionava | ✅ Resolt |
| 2 | [Cable motor Y mal connectat](eje-y-dual.md) | Motor Y no es movia | ✅ Resolt |
| 3 | [Ventilador hotend no s'apagava](ventilador-hotend.md) | Ventilador sempre encès | ✅ Resolt |
| 4 | [Endstop Z — configuració incorrecta](endstop-z.md) | Z no parava al tope superior | ✅ Resolt |
| 5 | [CR Touch — z_offset obligatori](crtouch-z-offset.md) | Klipper no arrencava | ✅ Resolt |
| 6 | [Termistor llit no instal·lat](termistor-cama.md) | Temperatura llit incorrecta | ⚠️ Pendent |
| 7 | [Fusell d'1,2m difícil de trobar](husillo-trapezoidal.md) | Coll d'ampolla en la construcció | ✅ Resolt |
| 8 | [Calefactor pujava sol a 220°C](calentador-runaway.md) | Risc de thermal runaway | ✅ Resolt |

---

## Lliçons apreses

1. **Provar el hardware abans de muntar-lo** — el slot MOTOR 3 estava defectuós de fàbrica. Sempre testejar els slots amb un motor i un driver abans d'assumir que funciona.

2. **El cable sempre és sospitós** — quan un motor no es mou, abans de canviar la configuració, comprova el cablejat físic.

3. **UART vs SPI no és intercanviable** — TMC2209 i TMC5160 usen protocols completament diferents. Barrejar-los a la configuració causa errors silenciosos difícils de depurar.

4. **Klipper exigeix `z_offset` a bltouch abans d'arrencar** — afegiu `z_offset: 0` temporalment i calibreu després amb `PROBE_CALIBRATE`.

5. **El llit calefactat necessita un termistor real** — usar `min_temp: -110` com a pedaç serveix per provar, però no dona informació real de temperatura.

6. **`dir_pin: !PF0`** — el `!` abans del pin inverteix la direcció. Quan un motor gira al revés, afegiu o traieu el `!` en lloc de moure els cables.
