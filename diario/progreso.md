# Diari del projecte

> Cronologia completa: des dels primers components al febrer fins a la impressora d'1mÂ³ funcionant.

---

## Fase 0 â€” RecepciÃ³ de components i primeres inspeccions (Febrer 2026)

### 25 de febrer â€” Arriba l'Octopus Pro

Vam rebre la BTT Octopus Pro V1.1 i la vam inspeccionar abans de connectar res.

![Part posterior Octopus Pro](../fotos/00-prototipo-inicial/IMG_20260225_190124.jpg)
*Vista posterior de la placa recentment rebuda. Es poden llegir clarament tots els slots: MOTOR0, MOTOR1, MOTOR2_1, MOTOR2_2... fins a MOTOR7. VersiÃ³: 1.1.*

TambÃ© vam inspeccionar la zona del USB-C i el connector de la font d'alimentaciÃ³.

**Primer pas:** Identificar tots els slots de driver i planificar quin motor va a cada slot abans de connectar res.

---

## Fase 1 â€” Muntatge de l'estructura (Febrerâ€“MarÃ§ 2026)

El primer pas real del projecte va ser tallar i muntar el **marc de perfils d'alumini item 40Ã—80mm**.

### Materials de l'estructura
- Perfils item 40Ã—80mm tallats a mida (vegeu [llista de materials](../hardware/lista-materiales-estructura.md))
- Connectors angulars metÃ lÂ·lics per unir els perfils a les cantonades
- Cargoleria M3 i M4 amb femelles T a la ranura del perfil

### Peces impreses per a l'estructura

Durant aquesta fase es van imprimir totes les peces en 3D necessÃ ries:

![Peces a l'aula](../fotos/03-piezas-impresas/piezas-impresas-todas-aula.jpeg)
*Totes les peces impreses llestes a l'aula: suports de motor verds i guies grises.*

![Suports grans NEMA23](../fotos/03-piezas-impresas/soportes-nema23-grandes.jpeg)
*Suports per als 4 motors NEMA 23 de l'eix Z.*

![Tensors i suports](../fotos/03-piezas-impresas/juntas-cruzadas-soportes.jpeg)
*Tensors de corretja GT2 i suports impresos.*

---

## Fase 2 â€” Primera electrÃ²nica: prova de la placa (Abril 2026)

### 8 d'abril â€” Primeres connexions

Vam connectar els primers components a l'Octopus Pro per provar que la placa funcionava abans de muntar-la a l'estructura.

![CR Touch ALT04](../fotos/02-electronica/IMG_20260408_193645.jpg)
*CR Touch model ALT04 amb els seus 5 cables de colors.*

![Cable hotend etiquetat](../fotos/02-electronica/cable-thermistor-heater-etiquetado.jpeg)
*Cables del hotend identificats: termistor ATC Semitec 104NT-4 i calentador cerÃ mic 24V 72W.*

![Octopus Pro vista general](../fotos/02-electronica/IMG_20260408_193713.jpg)
*Vista general de la placa amb drivers instalÂ·lats: TMC5160 vermells (Z) i TMC2209 blaus (X, Y, extrusor).*

### 10 d'abril â€” Primera arrencada amb CR Touch

Vam connectar el CR Touch a la placa i vam arrencar Klipper per primera vegada.

![Primer test electrÃ²nica](../fotos/05-montaje-electronica/primer-test-electronica.jpeg)
*Placa connectada a la font d'alimentaciÃ³ i al CR Touch per al primer test.*

**Problema trobat:** Klipper no arrencava perquÃ¨ faltava `z_offset: 0` a la secciÃ³ `[bltouch]`.  
â†’ Vegeu [crtouch-z-offset.md](../problemas/crtouch-z-offset.md)

### 10 d'abril â€” Test de motors

![ConnexiÃ³ CR Touch](../fotos/02-electronica/conexion-crtouch-cables.jpeg)
*Connectant els cables del CR Touch als pins PB6 i PB7 de l'Octopus Pro.*

---

## Fase 3 â€” Problemes de maquinari (Abril 2026)

### Slot MOTOR 3 defectuÃ³s

Vam descobrir que el slot MOTOR 3 de la placa estava **defectuÃ³s de fÃ brica**. El motor Z dret no responia.

**DiagnÃ²stic:** Vam provar el driver en altres slots â€” funcionava. Vam provar el motor en altres slots â€” funcionava. El slot MOTOR 3 estava mort.

**SoluciÃ³:** Moure el motor Z dret al slot **MOTOR 5** i actualitzar `printer.cfg`.

â†’ Vegeu [motor-z-slot-defectuoso.md](../problemas/motor-z-slot-defectuoso.md)

### Motor Y no es movia

El motor Y no responia. DiagnÃ²stic rÃ pid: **el cable JST no estava ben connectat**.

**LliÃ§Ã³:** Abans de canviar configuraciÃ³, sempre revisar el cable fÃ­sic.

â†’ Vegeu [eje-y-dual.md](../problemas/eje-y-dual.md)

---

## Fase 4 â€” Cablejat complet (23 d'abril 2026)

![Cablejat complet en procÃ©s](../fotos/05-montaje-electronica/cableado-completo-en-progreso.jpeg)
*SessiÃ³ de cablejat: tots els motors, sensors i calefactors connectats a la placa.*

![Setup complet amb Fluidd](../fotos/05-montaje-electronica/setup-octopus-cb1-fluidd.jpeg)
*Setup de treball: Octopus Pro + CB1 + teclat + pantalla amb Fluidd. La tauleta taronja Ã©s la pantalla KlipperScreen.*

### Problemes de ventilaciÃ³

El ventilador del dissipador del SO3 no s'apagava mai. La causa: estava configurat com a `[fan]` (ventilador de capa) en lloc de `[heater_fan]`.

â†’ Vegeu [ventilador-hotend.md](../problemas/ventilador-hotend.md)

---

## Fase 5 â€” Primera arrencada de Klipper

![Fluidd configuraciÃ³ inicial](../fotos/05-montaje-electronica/fluidd-config-error-pantalla.jpeg)
*Primera arrencada amb errors de configuraciÃ³. Fluidd mostra els avisos de printer.cfg que cal corregir abans de poder moure res. La tauleta taronja (KlipperScreen) ja estÃ  activa.*

![Fluidd funcionant](../fotos/05-montaje-electronica/fluidd-funcionando-primer-arranque.jpeg)
*Klipper funcionant! Fluidd mostrant la interfÃ­cie completa al monitor Dell. A l'esquerra el rack d'electrÃ²nica amb tot connectat.*

![Setup complet amb KlipperScreen](../fotos/05-montaje-electronica/fluidd-setup-klipperscreen-completo.jpeg)
*Setup de desenvolupament complet: monitor Dell amb Fluidd, KlipperScreen taronja a la taula i tota l'electrÃ²nica muntada a l'esquerra.*

Al monitor es veu la interfÃ­cie de Fluidd amb:
- Temperatura del hotend i la cama
- Controls de moviment XYZ
- Consola per a comandaments G-code
- Estat de la impressora

La tauleta taronja a la taula Ã©s la **BTT KlipperScreen** â€” pantalla tÃ ctil per controlar la impressora sense necessitat d'un PC.

---

## Fase 6 â€” Muntatge a l'estructura (Maig 2026)

### 22 d'abril â€” Guies lineals i carro

Vam instalÂ·lar les guies lineals MGN15R a l'eix Y:

![Guia lineal muntada](../fotos/01-estructura/IMG_20260422_173948.jpg)
*Guia MGN15R amb carro instalÂ·lada al perfil item 40Ã—80mm.*

![Suport motor amb politja](../fotos/01-estructura/soporte-motor-polea-perfil-aluminio.jpeg)
*Suport de motor imprÃ¨s (verd) muntat sobre el perfil item 40Ã—80mm. La politja lliure taronja guia la corretja GT2. A la part superior es veuen els tensors impresos tambÃ© en taronja.*

### 13 de maig â€” Cargol i eix Z

Vam instalÂ·lar els cargols trapezials M12 de 1200mm amb els seus suports impresos:

![Cargol i perfil](../fotos/01-estructura/IMG_20260513_175847.jpg)
*Cargol M12 (4 entrades, 8mm/rev) instalÂ·lat al perfil vertical de l'eix Z.*

![Suport cargol](../fotos/01-estructura/IMG_20260513_175857.jpg)
*Suport imprÃ¨s en blau per a l'extrem superior del cargol.*

---

## Fase 7 â€” IntegraciÃ³ final del capÃ§al

![Cables extrusor muntats](../fotos/01-estructura/cables-extrusor-etiquetados-montados.jpeg)
*Manat de cables del capÃ§al ja instalÂ·lat a la impressora. Els cables estan etiquetats com a "EXTRUSOR" per facilitar el manteniment.*

---

## Fase 8 â€” Cablejat complet i etiquetatge (1 juny 2026)

### 1 de juny â€” SessiÃ³ de cablejat final

SessiÃ³ completa de connexiÃ³ de tots els cables al panell d'electrÃ²nica verd. Es van etiquetar tots els connectors abans d'endollar.

**Sistema d'etiquetatge de cables:**

| Etiqueta | Cable |
|----------|-------|
| `MY1` | Motor Y esquerre (MOTOR2_1) |
| `MY2` | Motor Y dret (MOTOR2_2, paralÂ·lel) |
| `MX` | Motor X (MOTOR0) |
| `MZ1` | Motor Z esquerre (MOTOR1) |
| `MZ2` | Motor Z dret (MOTOR5) |
| `BLTOU` | Cable de control CR Touch (servo, PB6) |
| `NDSTOPZ` | Endstop Z mÃ xim (PF7) |
| `NDSTOPX` | Endstop X (PF5) |
| `BED` | Cables cama calefactada (pendent) |

![Panell electrÃ²nica complet](../fotos/05-montaje-electronica/panel-electronica-completo-verde.jpg)
*Panell verd amb Octopus Pro + CB1 completament cablejat. Tots els connectors porten etiquetes impreses.*

![Drivers i cables etiquetats](../fotos/05-montaje-electronica/drivers-tmc-cables-etiquetados.jpg)
*Detall dels drivers amb cables etiquetats sortint cap als motors.*

![KlipperScreen i electrÃ²nica](../fotos/05-montaje-electronica/klipperscreen-y-electronica-montados.jpg)
*La pantalla KlipperScreen (carcassa taronja impresa en 3D) instalÂ·lada al costat del panell d'electrÃ²nica a la mÃ quina.*

### Incident â€” Cable d'endstop trencat

Durant el procÃ©s d'etiquetatge es va trobar un cable d'endstop amb l'extrem pelat (sense terminal). Es va resoldar i es va crimpar un terminal nou.

**LliÃ§Ã³**: sempre revisar la continuÃ¯tat dels cables d'endstop abans d'energitzar.

---

## Fase 9 â€” MÃ quina completament assemblada (5 juny 2026)

La impressora estÃ  **dreta i operativa** al taller de l'Institut Jaume Huguet. Aquesta Ã©s la primera vegada que la mÃ quina es veu completament muntada a la seva ubicaciÃ³ final.

### Estat del muntatge

![MÃ quina completa â€” vista frontal](../fotos/08-maquina-completa/maquina-completa-frontal-hero.jpg)
*Impressora 3D de 1000Ã—1000Ã—1000mm completament assemblada al taller. AlÃ§ada total aproximada 1.5m. KlipperScreen (taronja) muntat al perfil lateral dret.*

![Interior â€” cama instalÂ·lada](../fotos/08-maquina-completa/interior-cama-instalada.jpg)
*Vista interior des de dalt: cama de vidre provisional instalÂ·lada sobre el marc inferior. Els cables de l'eix X recorren l'estructura.*

![Eix Y amb endstop](../fotos/08-maquina-completa/eje-y-carrusel-endstop.jpg)
*Carro de l'eix Y amb l'endstop (blau) muntat i el capÃ§al d'impressiÃ³ en posiciÃ³.*

![Vista des de l'aula](../fotos/08-maquina-completa/maquina-completa-lateral-aula.jpg)
*La impressora en el context de l'aula-taller. L'escala respecte al mobiliari de l'institut mostra la mida real de la mÃ quina.*

### Components muntats en aquesta fase
- âœ… Marc item 40Ã—80mm completament assemblat
- âœ… Guies lineals MGN15R instalÂ·lades a X, Y, Z
- âœ… Motors NEMA17 (X, YÃ—2) i NEMA23 (ZÃ—2) muntats
- âœ… Cargols M12 instalÂ·lats i acoblats
- âœ… Panell d'electrÃ²nica (Octopus Pro + CB1) instalÂ·lat
- âœ… KlipperScreen muntat al perfil lateral
- âœ… Cama de vidre provisional (per a primeres proves)
- âœ… Potes niveladoras instalÂ·lades
- â³ Cama calefactada (4 Ã— 500Ã—500mm) â€” pendent instalÂ·lar

---

## Estat actual

| Sistema | Estat |
|---------|-------|
| Marc estructura | âœ… Muntat |
| Guies lineals MGN15R | âœ… InstalÂ·lades |
| Eix X | âœ… Funcionant |
| Eix Y dual (2 motors paralÂ·lel) | âœ… Funcionant |
| Eix Z dual (TMC5160 + NEMA23) | âœ… Funcionant |
| Extrusor SO3 | âœ… Funcionant |
| CR Touch + Bed Mesh 5Ã—5 | âœ… Configurat |
| Z_TILT_ADJUST | âœ… Configurat |
| Klipper + Fluidd + KlipperScreen | âœ… Funcionant |
| Cama calefactada (4Ã—500Ã—500mm) | â³ Pendent instalÂ·lar |
| Arxius 3D de peces | â³ Pendent pujar |

---

## Pendents

- [ ] InstalÂ·lar termistor a la cama calefactada
- [ ] InstalÂ·lar 4 Ã— cames calefactades 500Ã—500mm en paralÂ·lel
- [ ] Pujar arxius STL/STEP de peces impreses
- [ ] Completar calibratge final (PID cama, Z offset definitiu amb `PROBE_CALIBRATE`)
- [ ] Afegir ventiladors al panell d'electrÃ²nica
