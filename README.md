<p align="center">
  <img src="assets/logo-institut-jaume-huguet.png" alt="Institut Jaume Huguet â€” Antiga Escola del Treball, Valls" width="500"/>
</p>

<h1 align="center">Impressora 3D Cartesiana 1000Ã—1000Ã—1000 mm</h1>

<p align="center">
  <strong>Cicle Formatiu de FabricaciÃ³ Additiva â€” CEFP 2025-2026</strong><br>
  <strong>Institut Jaume Huguet â€” Antiga Escola del Treball</strong>, Valls (Tarragona)<br>
  <a href="https://institutjaumehuguet.cat">institutjaumehuguet.cat</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Firmware-Klipper-brightgreen" alt="Klipper"/>
  <img src="https://img.shields.io/badge/Board-BTT%20Octopus%20Pro%20V1.1-blue" alt="BTT"/>
  <img src="https://img.shields.io/github/last-commit/xXSimon91Xx/impresora-3d-1000x1000-ca" alt="Last commit"/>
</p>

---

## Versions del repositori

| Idioma | Repositori |
|--------|------------|
| ðŸ‡ªðŸ‡¸ CastellÃ  | [impresora-3d-1000x1000](https://github.com/xXSimon91Xx/impresora-3d-1000x1000) |
| ðŸ´ó ¥ó ³ó £ó ´ó ¿ CatalÃ  | **aquest repositori** |
| ðŸ‡¬ðŸ‡§ English | [impresora-3d-1000x1000-en](https://github.com/xXSimon91Xx/impresora-3d-1000x1000-en) |

---

## Ãndex de documentaciÃ³

### Per comenÃ§ar â€” guia de la mÃ quina
- [QuÃ¨ Ã©s aquesta mÃ quina?](#quÃ¨-Ã©s-aquesta-mÃ quina) â€” resum accessible per a tots els nivells
- [L'equip](equipo.md) â€” alumnes, professors i agraÃ¯ments
- [Diari del projecte](diario/progreso.md) â€” cronologia completa des del febrer de 2026

### Hardware
- [**Arxius 3D â€” 57 arxius STEP**](hardware/archivos-3d/) â€” ensamblatge complet, extrusor, eixos, llit
- [Llista de materials â€” Estructura](hardware/lista-materiales-estructura.md)
- [PlÃ nols estructurals â€” Marc item](hardware/planos-estructurales.md)
- [Placa BTT Octopus Pro](hardware/placa-octopus-pro.md)
- [Motors i drivers](hardware/motores-drivers.md)
- [Peces impreses en 3D](hardware/piezas-impresas.md)
- **Cablejat:**
  - [VisiÃ³ general â€” tots els sistemes](hardware/cableado/overview.md)
  - [Eix X](hardware/cableado/eje-x.md) Â· [Eix Y dual](hardware/cableado/eje-y.md) Â· [Eix Z dual](hardware/cableado/eje-z.md)
  - [Extrusor Smart Orbiter v3.0](hardware/cableado/extrusor.md)
  - [CR Touch â€” Sonda Z](hardware/cableado/crtouch.md)
  - [Llit calefactat](hardware/cableado/cama.md)

### Firmware
- [ConfiguraciÃ³ completa (printer.cfg)](firmware/printer.cfg)
- [InstalÂ·laciÃ³ de Klipper al CB1](firmware/klipper-setup.md)
- [CalibraciÃ³ â€” PID, Z offset, Bed Mesh](firmware/calibracion.md)

### Problemes trobats i solucions
- [Ãndex de problemes](problemas/README.md)

### Idees de futur
- [Projectes i millores planificades](futuro.md)

---

## QuÃ¨ Ã©s aquesta mÃ quina?

Aquesta impressora 3D pot fabricar objectes de fins a **1 metre de llarg, 1 metre d'ample i 1 metre d'alt**. Ã‰s una de les impressores mÃ©s grans que es poden construir amb peces comercials disponibles.

Es tracta d'una impressora **cartesiana**: el capÃ§al d'impressiÃ³ es mou als eixos X i Y, i l'estructura puja i baixa a l'eix Z. El material (filament de plÃ stic) es fon i es diposita capa a capa fins a formar la peÃ§a.

**Per a quÃ¨ serveix?** Per fabricar peces grans que no caben en una impressora normal: estructures, motlles, prototips industrials, peces de cotxes, projectes d'arquitectura, etc.

**QuÃ¨ la fa especial?** Tot el sistema de control (firmware Klipper) Ã©s de codi obert i s'hi pot accedir des de qualsevol dispositiu de la xarxa de l'institut. No necessita un ordinador connectat per imprimir â€” tÃ© el seu propi ordinador de control integrat (BTT CB1).

---

## La mÃ quina acabada â€” 5 juny 2026

![MÃ quina completa](fotos/08-maquina-completa/maquina-completa-frontal-hero.jpg)
*La impressora 3D de 1000Ã—1000Ã—1000mm completament ensamblada al taller de l'Institut Jaume Huguet. AlÃ§ada total ~1,5m. KlipperScreen tÃ ctil (taronja) muntat al perfil dret. Llit de vidre provisional instalÂ·lat.*

![MÃ quina vista frontal amb pantalla](fotos/08-maquina-completa/maquina-completa-frontal-clase.jpg)
*Vista frontal de la impressora a l'aula del taller. L'escala respecte al mobiliari de l'institut dona una idea de la mida de la mÃ quina.*

---

## Fotos del projecte

### Panell d'electrÃ²nica instalÂ·lat â€” 1 juny 2026

![Panell electrÃ²nica complet](fotos/05-montaje-electronica/panel-electronica-completo-verde.jpg)
*Panell verd amb la BTT Octopus Pro V1.1, el CB1 i tots els cables etiquetats i connectats. El panell es desmunta per a manteniment.*

![KlipperScreen i electrÃ²nica muntats](fotos/05-montaje-electronica/klipperscreen-y-electronica-montados.jpg)
*La pantalla tÃ ctil KlipperScreen (taronja) al costat del panell d'electrÃ²nica ja instalÂ·lats a la mÃ quina.*

![Drivers TMC i cables etiquetats](fotos/05-montaje-electronica/drivers-tmc-cables-etiquetados.jpg)
*Detall dels drivers TMC2209 (blau) i TMC5160 (vermell) amb els cables etiquetats: MY1, MY2, MX, MZ1, MZ2, EXTRUSOR.*

### Primer arrencada amb Klipper

![Fluidd funcionant](fotos/05-montaje-electronica/fluidd-funcionando-primer-arranque.jpeg)
*Primera arrencada amb Fluidd al monitor Dell, KlipperScreen a la pantalla taronja, i tota l'electrÃ²nica connectada.*

### Placa base â€” BTT Octopus Pro V1.1

![Octopus Pro xip STM32](fotos/05-montaje-electronica/stm32h723-drivers-closeup.jpg)
*Detall del xip STM32H723 de l'Octopus Pro amb els drivers TMC5160 (vermell, eix Z) i TMC2209 (blau, eixos XY i extrusor).*

![Octopus Pro amb drivers](fotos/02-electronica/IMG_20260408_193713.jpg)
*Vista frontal de la BTT Octopus Pro V1.1. TMC5160 vermells per a Z (alta intensitat), TMC2209 blaus per a X, Y i extrusor.*

### Estructura i guies lineals

![Guia lineal](fotos/01-estructura/IMG_20260422_173948.jpg)
*Guia lineal MGN15R amb carro muntat sobre perfil item 40Ã—80mm.*

### Peces impreses en 3D

![Peces a l'aula](fotos/03-piezas-impresas/piezas-impresas-todas-aula.jpeg)
*Totes les peces impreses abans del muntatge: suports de motor (verd PETG), guies i clips (gris ASA).*

### PlÃ nols estructurals â€” marcs item

![PlÃ nol item isomÃ¨tric](fotos/07-planos-item/plano-item-p11-vista-isometrica.jpg)
*Vista isomÃ¨trica del marc d'alumini. PlÃ nols generats amb el configurador d'item â€” 17 pÃ gines.*

---

## Especificacions tÃ¨cniques

| ParÃ metre | Valor |
|-----------|-------|
| **Volum d'impressiÃ³** | 1000 Ã— 1000 Ã— 1000 mm |
| **CinemÃ tica** | Cartesiana (eixos X/Y/Z independents) |
| **Firmware** | Klipper + Fluidd (interfÃ­cie web) + KlipperScreen |
| **Placa base** | BigTreeTech Octopus Pro V1.1 (STM32H723) |
| **Ordinador de control** | BigTreeTech CB1 (ARM Cortex-A55, Linux) |
| **Velocitat mÃ xima** | 300 mm/s (XY), 5 mm/s (Z) |
| **AcceleraciÃ³ mÃ xima** | 3000 mm/sÂ² |
| **Extrusor** | LDO Smart Orbiter v3.0 (direct drive, reducciÃ³ 7,5:1) |
| **Motor extrusor** | LDO-36STH20-1004AHG |
| **Hotend** | Broquet 0,4 mm, calefactor cerÃ mic 24V 72W |
| **Sensor Z** | CR Touch ALT04 (Creality) |
| **Drivers XY/Extrusor** | TMC2209 (UART, StealthChop) |
| **Drivers Z** | TMC5160-T Pro (SPI, alta intensitat, 2A) |
| **Motors XY** | NEMA 17 |
| **Motors Z** | NEMA 23 (2 unitats, Z dual independent) |
| **Guies lineals** | MGN15R (rodaments recirculants) |
| **Fusell Z** | M12 Ã— 4 entrades â†’ 8 mm/volta |
| **Marc** | Perfils item 40Ã—80 mm (sÃ¨rie XMS, art. 0.0.669.99) |
| **Peces mecÃ niques** | PETG (verd, no crÃ­tic) + ASA/ABS (gris, crÃ­tic) |

---

## HistÃ²ria del projecte

Aquest projecte va ser dissenyat des del principi com una impressora de **1000Ã—1000Ã—1000 mm**, amb l'objectiu que la mÃ quina quedi a l'**Institut Jaume Huguet** per a Ãºs dels alumnes i com a referÃ¨ncia perquÃ¨ altres centres la puguin replicar.

Abans de construir-la, vam usar una impressora de 500Ã—500 mm per validar el firmware i l'electrÃ²nica â€” aixÃ­ vam arribar a la impressora gran amb tot ja provat.

El repte tÃ¨cnic mÃ©s gran va ser el **doble eix Z amb NEMA 23**: necessiten 2 A de corrent, la qual cosa obliga a usar drivers TMC5160 (els TMC2209 normals no aguanten). A mÃ©s, un dels slots de la placa va resultar estar defectuÃ³s de fÃ brica i es va haver de moure el motor al slot MOTOR 5.

El marc estÃ  construÃ¯t amb **perfils item 40Ã—80 mm**, un sistema modular d'alumini tÃ¨cnic usat en maquinÃ ria industrial, que garanteix rigidesa i precisiÃ³ fins i tot a aquesta escala.

---

*Cicle Formatiu de FabricaciÃ³ Additiva â€” CEFP 2025-2026 | Institut Jaume Huguet â€” Antiga Escola del Treball, Valls*
