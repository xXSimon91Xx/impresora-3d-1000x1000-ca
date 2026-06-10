# Llista de components (BOM)

> Per què vam triar cada peça — el raonament darrere de cada decisió.

---

## Electrònica principal

### BTT Octopus Pro V1.1 (STM32H723)

| Camp | Valor |
|------|-------|
| **MCU** | STM32H723ZET6 (ARM Cortex-M7, 550 MHz) |
| **Slots de driver** | 8 (usem 5: X, Y, Z, Z1, extrusor) |
| **Protocol drivers** | UART (TMC2209) i SPI (TMC5160) simultani |
| **Entrades temperatura** | 4 termistors + 2 PT100/PT1000 |
| **Sortides calefactor** | 4 (usem 2: hotend + llit) |
| **Sortides ventilador** | 6 PWM + 2 sempre encesos |
| **Alimentació motors** | 12–60V independent |
| **Preu aproximat** | ~60€ |

**Per què la vam triar?**  
Necessitàvem una placa amb capacitat per a **8 drivers** (el format 1×1m té múltiples motors), suport simultani per a TMC2209 (UART) i TMC5160 (SPI), i compatibilitat nativa amb Klipper. L'Octopus Pro compleix tot això i té una comunitat enorme.

---

### BTT CB1 (ordinador de control)

| Camp | Valor |
|------|-------|
| **CPU** | ARM Cortex-A55 (quad-core) |
| **RAM** | 1 GB DDR4 |
| **Emmagatzematge** | microSD |
| **Sistema operatiu** | Armbian + Klipper |
| **Interfície web** | Fluidd |
| **Connexió amb Octopus** | USB-C |

**Per què no una Raspberry Pi?**  
El CB1 és més barat, munta directament a l'Octopus Pro (sense cables addicionals) i ve preconfigurada per a Klipper. També és més fàcil d'aconseguir el 2025-2026 que una RPi.

---

## Drivers de motor

### TMC2209 (UART) — per a X, Y i extrusor

| Paràmetre | Valor |
|-----------|-------|
| **Corrent màxim** | 2A (run), 1.2A recomanat continu |
| **Protocol** | UART (1 sol cable de dades) |
| **StealthChop** | Sí (mode silenciós) |
| **Detecció de càrrega** | StallGuard 4 |
| **Color a la nostra placa** | Blau |

Usem TMC2209 per als eixos que usen **NEMA 17**: velocitat suficient, baix consum, mode silenciós perfecte.

### TMC5160-T Pro (SPI) — per a Z esquerra i Z dreta

| Paràmetre | Valor |
|-----------|-------|
| **Corrent màxim** | 4.4A de pic, 3A RMS |
| **Protocol** | SPI (4 cables) |
| **StealthChop** | Sí |
| **Tensió** | 8–60V |
| **Color a la nostra placa** | Vermell |

**Per què TMC5160 per a Z i no TMC2209?**  
Els motors Z són **NEMA 23**, que necessiten **2A de corrent continu**. El TMC2209 només aguanta ~1.2A de forma fiable. Si usàvem TMC2209 a Z, els motors s'escalfarien, perdrien passos, o el driver moriria.

---

## Motors

### NEMA 17 — Eixos X, Y (×3) i extrusor (×1)

- Pas: 1.8° (200 passos/volta)
- Corrent run: 0.6A (X), 1.0A (Y), 0.85A (extrusor)
- Volum 1m: necessitem **2 motors a Y** en paral·lel (vegeu [eix Y](cableado/eje-y.md))

### NEMA 23 — Eix Z (×2)

- Major força d'empenta per sostenir el llit d'1m²
- Pas: 1.8°
- Corrent run: 2.0A (requereix TMC5160)
- **Z dual independent**: cada motor té el seu propi driver per poder fer `Z_TILT_ADJUST`

### LDO-36STH20-1004AHG — Extrusor (dins del SO3)

- Motor compacte integrat al Smart Orbiter v3.0
- Corrent run: 0.85A

---

## Extrusor — LDO Smart Orbiter v3.0

| Camp | Valor |
|------|-------|
| **Model** | LDO Smart Orbiter v3.0 (dissenyat pel Dr. Robert Lőrincz) |
| **Tipus** | Direct drive amb reducció 7.5:1 |
| **rotation_distance calibrat** | 4.637 mm/rev |
| **Motor** | LDO-36STH20-1004AHG |
| **Sensor de filament** | Integrat |
| **Botó de descàrrega** | Integrat |

**Per què el Smart Orbiter v3.0?**  
Alta relació de reducció (7.5:1), molt lleuger per a un direct drive, sensor de filament integrat i botó de descàrrega integrat. És un dels extrusors direct drive més recomanats per a impressores de gran format.

---

## Sonda de nivellament — CR Touch (ALT04)

| Camp | Valor |
|------|-------|
| **Model** | Creality CR Touch ALT04 |
| **Tipus** | Punta metàl·lica servo-controlada |
| **Protocol** | 5 fils (compatible BL-Touch) |
| **Repetibilitat** | ±0.005 mm |

**Per què CR Touch i no un final de carrera a Z?**  
Amb un llit d'1m² és impossible nivelar-lo perfectament a mà. El CR Touch + Klipper `BED_MESH_CALIBRATE` mapeja 25 punts (5×5) i compensa les imperfeccions del llit automàticament durant la impressió.

---

## Estructura mecànica

| Component | Especificació |
|-----------|---------------|
| **Perfils d'alumini** | item 40×80mm, sèrie XMS, art. 0.0.669.99 |
| **Guies lineals** | MGN15R (carros R = rodaments de boles recirculants) |
| **Fusell Z** | Mètric 12, pas 2mm, 4 entrades (= 8mm/volta) |
| **Corretja XY** | GT2, pas 2mm, politja 20 dents (= 40mm/volta) |

**Per què MGN15R i no varetes llises?**  
Per a 1m de recorregut, les varetes llises tendeixen a flectar al centre. Les guies MGN15R són més rígides i precises.

**Per què fusell M12 (mètric 12, 4 entrades)?**  
- `rotation_distance = pas × entrades = 2mm × 4 = 8mm/volta`
- Major avanç per volta que T8 → menys passos per mm, però més velocitat Z
- Suficient per a una impressora (no necessitem velocitat extrema a Z)

---

## Enllaços de compra

- [BTT Octopus Pro V1.1 + CB1 (Amazon ES)](https://www.amazon.es/dp/B0BN7B6KV6)
- [Component 2 (Amazon ES)](https://www.amazon.es/dp/B0CLLRG551)
- [Component 3 (Amazon ES)](https://www.amazon.es/dp/B0D97P6Z64)
- [BTT Octopus Pro — Repositori oficial](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro)
