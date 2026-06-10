# Projectes de futur

> Idees i millores planificades per continuar el desenvolupament de la màquina.  
> Algunes són senzilles d'implementar; d'altres són projectes complets en si mateixos.

---

## Millores mecàniques

### 4 capçals d'impressió amb canvi de color
Implementar un sistema de múltiples extrusors (4 capçals) per a impressió multicolor o multi-material en una sola peça. Requereix modificacions al firmware (Klipper admet multi-extrusor de manera nativa) i el disseny d'un capçal intercanviable o de tipus "eina".

### 4 fusos Z (de 2 a 4)
Afegir dos fusos addicionals a l'eix Z per millorar la rigidesa del sistema en una màquina d'aquesta mida. Amb 4 punts de suport es redueix la deflexió i es pot usar `Z_TILT_ADJUST` amb més punts de sondeig.

### Sistema de cadenes portacables
Substituir els cables lliures actuals per cadenes portacables (cable chains) als eixos X i Y. Millora la durabilitat del cablejat i dona un aspecte més professional.

### Guies lineals amb lubricació automàtica
Instal·lar un sistema automàtic d'engràs de les guies MGN15R. Existeixen dispositius comercials amb tubs i dipòsit que apliquen oli periòdicament. Augmenta la vida útil de les guies i redueix el manteniment manual.

---

## Millores de materials

### Placa PEI en lloc de vidre
Substituir la superfície d'impressió de vidre per una placa PEI magnètica. El PEI ofereix millor adherència per a PETG, ABS i ASA, i permet treure les peces amb un simple doblegat de la làmina.

### Enclosure per a ABS/ASA/PP
Per poder imprimir amb ABS, ASA o polipropilè de forma consistent, la màquina necessita una carcassa tancada que mantingui la temperatura ambient elevada i eviti els corrents d'aire. Els panells de policarbonat corrugat amb imants de neodimi ja estan planificats (vegeu [peces impreses](hardware/piezas-impresas.md)).

### Extrusor de pellets
Dissenyar o adaptar un extrusor que funcioni directament amb pellets de plàstic en lloc de filament enrotllat. Redueix el cost del material i obre la possibilitat d'usar plàstic reciclat.

### Broquet 1,5 mm per a impressió ràpida
Per a peces grans on el detall no és crític, un broquet d'1,5 mm accelera enormement els temps d'impressió. Actualment la màquina té un broquet de 0,4 mm.

---

## Millores electròniques i de control

### Encoders als motors
Afegir encoders als motors pas a pas per passar a control en bucle tancat. Elimina la pèrdua de passos i permet recuperar la posició si hi ha un obstacle. Especialment rellevant en una màquina tan gran on els errors acumulats són majors.

### Sistema de recuperació de tall de corrent
Implementar i provar la macro de power-loss recovery de Klipper. Quan se'n va la llum, la posició es guarda a la SD; quan torna el corrent, la impressió pot continuar des d'on es va quedar.

> **Nota tècnica**: l'eix Z no baixa quan es talla el corrent perquè els fusos trapezoidals són auto-frenants per fricció. No obstant, amb peces molt pesades sobre el capçal podria baixar lleugerament.

### LEDs d'estat de la màquina
- 🟢 **Verd**: tot bé, imprimint normalment
- 🔴 **Vermell fix**: error crític (temperatura, endstop)
- 🟡 **Groc intermitent**: avís (filament esgotat, temperatura fora de rang)

Implementable amb una tira LED WS2812B controlada des de Klipper amb `[neopixel]`.

### Més botons de parada d'emergència
Afegir polsadors de parada d'emergència addicionals en diferents punts de la màquina (actualment només n'hi ha un). Per a una màquina d'1m³ operada en un taller escolar, és important poder aturar la màquina des de qualsevol posició.

### Magnetotèrmic (protecció de sobrecàrrega)
Instal·lar un interruptor magnetotèrmic a la línia d'alimentació de la màquina. Protegeix contra sobrecàrregues de corrent i curtcircuits, especialment important quan s'instal·li el llit calefactat d'alta potència (4 × 500×500mm en paral·lel).

### Ventiladors a la caixa d'electrònica
Afegir ventiladors d'extracció al panell verd on es troben l'Octopus Pro i el CB1. Amb el llit calefactat en funcionament i tots els drivers carregats, la temperatura dins del panell pot pujar considerablement.

---

## Millores de programari i documentació

### Marcatge CE
Perquè la màquina pugui ser usada oficialment en qualsevol centre educatiu o cedida a altres instituts, necessitaria obtenir el marcatge CE que acredita el compliment de les directives europees de seguretat elèctrica i electromagnètica.

### Versions del repositori en català i anglès
Traduir tota la documentació al català (per a ús local) i a l'anglès (perquè altres projectes internacionals puguin replicar la màquina).

### Render 3D explotat de la màquina
Crear un render amb vista explosionada de tots els components perquè sigui fàcil entendre l'ensamblatge sense tenir la màquina davant. Pendent quan es pugin els arxius STEP del marc.

---

## Pendents immediats

- [ ] Instal·lar 4 × llits calefactats 500×500mm en paral·lel
- [ ] Pujar arxius STL i STEP de peces impreses
- [ ] Pujar plànols del marc en PDF
- [ ] Calibració final: PID llit, Z offset definitiu amb `PROBE_CALIBRATE`
- [ ] Provar i validar macro de power-loss recovery
