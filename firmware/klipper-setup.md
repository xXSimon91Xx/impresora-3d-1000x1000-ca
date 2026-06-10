# Instal·lació i configuració de Klipper

> Guia per instal·lar Klipper al BTT CB1 i flashejar el firmware a la BTT Octopus Pro V1.1 (STM32H723).

---

## Arquitectura del sistema

```
CB1 (ARM Linux) ─── USB-C ─── Octopus Pro (STM32H723)
      │                              │
      │ Klipper host                 │ Firmware Klipper MCU
      │ Fluidd (interfície web)      │ Gestiona: motors, temp, ventiladors
      │                              │
  microSD                        Flash interna
```

El CB1 executa el **host de Klipper** (Python) i es comunica amb el **firmware de Klipper** al STM32 de l'Octopus Pro via USB. Fluidd és la interfície web per controlar la impressora des del navegador.

---

## Pas 1: Imatge del CB1

1. Descarregar la imatge del BTT CB1 amb Klipper preinstal·lat des del repositori oficial de BigTreeTech
2. Flashejar la microSD amb **Balena Etcher** o **Raspberry Pi Imager**
3. Inserir la microSD al slot del CB1
4. La imatge ja inclou: Klipper + Moonraker + Fluidd + KlipperScreen

---

## Pas 2: Compilar i flashejar el firmware per al STM32H723

### Configuració de compilació

```bash
cd ~/klipper
make menuconfig
```

Al menú seleccionar:
- **Micro-controller Architecture:** STMicroelectronics STM32
- **Processor model:** STM32H723
- **Bootloader offset:** 128KiB bootloader
- **Clock Reference:** 25 MHz crystal
- **Communication interface:** USB (on PA11/PA12)

```bash
make clean
make
```

Això genera `out/klipper.bin`.

### Mètode de flasheig: targeta SD

1. Reanomenar `klipper.bin` a `firmware.bin`
2. Copiar a una microSD formatada en FAT32
3. Inserir la SD al slot de l'**Octopus Pro** (no al CB1)
4. Engegar la placa — el bootloader carrega el firmware automàticament
5. El LED parpelleja durant el flasheig, després queda fix

### Verificar que el firmware ha carregat

```bash
ls /dev/serial/by-id/
# Ha d'aparèixer: usb-Klipper_stm32h723xx_XXXX-if00
```

---

## Pas 3: Configurar printer.cfg

L'arxiu de configuració principal es troba a:
```
~/printer_data/config/printer.cfg
```

Copiar el `printer.cfg` d'aquest repositori com a punt de partida.

Actualitzar el serial del MCU:
```ini
[mcu]
serial: /dev/serial/by-id/usb-Klipper_stm32h723xx_XXXXXXXX-if00
```
(L'ID exacte varia per placa — usar `ls /dev/serial/by-id/` per obtenir-lo)

---

## Pas 4: Accedir a Fluidd

Un cop tot connectat:

1. L'Octopus Pro connectada al CB1 per USB-C
2. El CB1 connectat a la xarxa WiFi
3. Obrir el navegador i accedir a la IP del CB1 (ex: `http://192.168.1.XX`)

Fluidd mostra:
- Estat de la impressora en temps real
- Temperatura del hotend i del llit
- Controls de moviment manual
- Consola per enviar comandes G-code

---

## Comandes de prova inicial

```gcode
# Verificar temperatura (sense escalfar)
M105

# Moure eix X sense fer home (per provar)
SET_KINEMATIC_POSITION X=100 Y=100 Z=10
G1 X200 F3000

# Test d'extrusor (forçat, hotend fred)
SET_KINEMATIC_POSITION X=100 Y=100 Z=10
# (amb min_extrude_temp: 0 temporalment)

# Home complet
G28

# Calibració Z amb CR Touch
PROBE_CALIBRATE

# Z_TILT (igualar els dos motors Z)
Z_TILT_ADJUST

# Mapa de llit
BED_MESH_CALIBRATE
```

---

## Estructura d'arxius de Klipper al CB1

```
~/printer_data/
├── config/
│   ├── printer.cfg          # Configuració principal
│   └── moonraker.conf       # Config de l'API web
├── gcodes/                  # Arxius G-code per imprimir
├── logs/                    # Registres del sistema
└── database/                # Base de dades de Klipper
```
