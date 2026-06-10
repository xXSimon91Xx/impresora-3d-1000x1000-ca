# Diagrama general de cablejat

> Vista simplificada de totes les connexions elèctriques del sistema.

---

## Diagrama de blocs

```
┌─────────────────────────────────────────────────────────────────────┐
│                    FONT D'ALIMENTACIÓ 24V                           │
└──────────┬───────────────────────┬──────────────────────────────────┘
           │                       │
    MOTOR-POWER + POWER         BED-POWER
           │                       │
           ▼                       ▼
┌──────────────────────────────────────────────────────────────────────┐
│                   BTT OCTOPUS PRO V1.1 (STM32H723)                  │
│                                                                      │
│  MOTOR0──TMC2209──► Motor X (NEMA17)                                │
│  MOTOR2_1 TMC2209──► Motor Y esq (NEMA17) ┐ En paral·lel           │
│  MOTOR2_2─────────► Motor Y dret (NEMA17) ┘                        │
│  MOTOR1──TMC5160──► Motor Z esq (NEMA23)                            │
│  MOTOR5──TMC5160──► Motor Z dret (NEMA23)  [MOTOR3 = DEFECTUÓS]    │
│  MOTOR4──TMC2209──► Extrusor SO3 (LDO)                             │
│                                                                      │
│  STOP0 (PG6) ◄──── Endstop X                                       │
│  STOP2 (PG9) ◄──── Endstop Y                                       │
│  T3 (PF7)    ◄──── Endstop Z màxim (seguretat)                     │
│  PB6 / PB7   ◄──► CR Touch (control + sensor)                      │
│                                                                      │
│  T0 (PF4)    ◄──── Termistor hotend (ATC Semitec 104NT-4)          │
│  T1 (PF3)    ◄──── Termistor llit (pendent d'instal·lar)           │
│  HE0 (PA3)   ────► Calefactor hotend 24V 72W                       │
│  HE_BED(PA1) ────► Llit calefactat 24V                             │
│  FAN2 (PD12) ────► Ventilador heatsink SO3                         │
│                                                                      │
│  PG11 ◄────────── Sensor filament SO3                               │
│  PG10 ◄────────── Botó descàrrega SO3                              │
│                                                                      │
│  USB-C ◄─────────► BTT CB1 (ARM, Klipper host, Fluidd web UI)      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## CR Touch — 5 cables (detall)

```
CR Touch ALT04
  ┌─────────────┐
  │ Blau   PB7  │── Sensor (senyal de toc) → ^ pullup
  │ Vermell GND │── Massa
  │ Groc   PB6  │── Control servo (deploy / retract)
  │ Negre  5V   │── Alimentació 5V
  │ Blanc  GND  │── Massa
  └─────────────┘
```

![CR Touch cables](../../fotos/02-electronica/IMG_20260408_193645.jpg)
*CR Touch ALT04 amb el seu cable de 5 colors.*

---

## TMC5160 — Bus SPI compartit

Els dos drivers TMC5160 (Z esq i Z dret) comparteixen el mateix bus SPI però cada un té el seu propi **Chip Select**:

```
Bus SPI (compartit):
  PA6 ──── MISO  ──── TMC5160 Z esq ──── TMC5160 Z dret
  PA7 ──── MOSI
  PA5 ──── SCLK

Chip Select (individual):
  PD11 ──── CS del TMC5160 Z esq (MOTOR 1)
  PE4  ──── CS del TMC5160 Z dret (MOTOR 5)
```

---

## Jumpers de tensió a l'Octopus Pro

**Important**: Els TMC5160 necessiten el jumper en posició `VFused` (tensió del motor), no a 5V.

```
Cada slot de driver té 3 jumpers de tensió:
  [VFused] [VFused] [VFused]  ← posició correcta per a TMC5160 i TMC2209
```

---

## Cable del hotend (etiquetat)

![Cable hotend etiquetat](../../fotos/02-electronica/cable-thermistor-heater-etiquetado.jpeg)
*Cable de l'extrusor amb etiquetes: "Thermistor104NT-4" i "24V/72W Heater".*

```
Connector hotend → Octopus Pro:
  Termistor (2 cables blancs) ──── T0 (PF4)
  Calefactor (2 cables negres) ──── HE0 (PA3)
```

---

## Cables de motor (NEMA17 i NEMA23)

Els motors pas a pas tenen 4 cables que formen 2 bobines:

```
Motor → Connector JST 4 pins a la placa:
  A+  ──── pin 1
  A-  ──── pin 2
  B+  ──── pin 3
  B-  ──── pin 4

⚠ Si el motor vibra sense moure's: A+ i A- estan intercanviats
⚠ Si gira al revés: canviar ! a dir_pin (o intercanviar A+ amb B+)
```

---

## Cables "EXTRUSOR" etiquetats a la màquina

![Cables extrusor muntats](../../fotos/01-estructura/cables-extrusor-etiquetados-montados.jpeg)
*Manat de cables del capçal etiquetats com "EXTRUSOR" ja muntats a la impressora.*

Els cables del capçal inclouen: termistor, calefactor, motor extrusor, ventilador, sensor filament, botó descàrrega i CR Touch.

---

## Diagrama d'alimentació

```
Font 24V
  ├── MOTOR-POWER ──── Tots els slots de driver (MOTOR 0-7)
  ├── POWER       ──── Lògica + ventiladors + calefactor hotend
  └── BED-POWER   ──── Llit calefactat (rail separat)
```

> **Per què separar BED-POWER:** El llit consumeix molt de corrent (pot ser 10-20A). Si comparteix rail amb els motors, els pics de corrent poden causar fallades de pas o soroll als senyals.
