# Llista de materials — Estructura mecànica

> **Nota:** Aquesta llista es basa en els plànols del projecte. Per a mesures exactes consulteu el [PDF de plànols estructurals](planos-estructurales.md) — generat amb el configurador d'item.

---

## Perfils d'alumini item (40×80mm)

> Els perfils són de la marca **item** (Industrietechnik), sèrie XMS, art. 0.0.669.99.  
> Perfil X 8 R40-90°, secció 40×80 mm, ranura 8 mm, forat D6.8 M8 als extrems.

| Peça | Quantitat | Longitud | Eix |
|------|-----------|----------|-----|
| Perfil tipus 13v (frontal/posterior) | 3 | 1400 mm | Y |
| Perfil tipus 14v (lateral) | 1 | 1400 mm | X |
| Perfils verticals (columnes) | 4 | 1400 mm | Z |
| Perfils de reforç interior | ~4-8 | variable | — |

> Dimensió exterior del marc: **1441.5 mm** (1400 mm sense accessoris). Vegeu [plànols estructurals](planos-estructurales.md).

---

## Guies lineals MGN15R

| Eix | Quantitat | Longitud |
|-----|-----------|---------|
| X | 1 | ~1050mm |
| Y | 2 | ~1050mm |
| Z (vertical) | 2 | ~1100mm |

Carros inclosos amb cada guia: tipus R (rodaments de boles recirculants).

---

## Fusos trapezoidals (Eix Z)

| Especificació | Valor |
|---------------|-------|
| Diàmetre | M12 (mètric 12) |
| Pas | 2mm |
| Entrades | 4 (pas efectiu = 8mm/volta) |
| Longitud | ≥1200mm |
| Quantitat | 2 (un per motor Z) |

---

## Motors pas a pas

| Motor | Quantitat | Eix | Model |
|-------|-----------|-----|-------|
| NEMA 17 | 3 | X (×1) + Y (×2) | Estàndard 42mm |
| NEMA 23 | 2 | Z esq + Z dret | 57mm, ≥2A |
| LDO-36STH20 (integrat SO3) | 1 | Extrusor | LDO especial |

![Motors en caixa](../fotos/02-electronica/motores-nema-en-caja.jpeg)
*Motors NEMA arribant en caixa amb cargoleria inclosa.*

---

## Corretges i politges (eixos X i Y)

| Component | Especificació | Quantitat |
|-----------|---------------|-----------|
| Corretja GT2 | pas 2mm, amplada 6mm | ~4m total (2m per eix) |
| Politja GT2 motriu | 20 dents, eix 5mm | 3 (X + 2×Y) |
| Tensor / politja lliure | GT2 sense dents | ×4-6 |

---

## Cargoleria

> Llista aproximada. Vegeu el PDF de plànols per a quantitats exactes.

| Cargol | Quantitat |
|--------|-----------|
| M3×8 cap cilíndric | ~100 |
| M3×12 | ~50 |
| M4×8 | ~80 |
| M4×12 | ~40 |
| Femelles T M4 (per a ranura perfil item 8mm) | ~100 |
| Inserts M3 (per a peces impreses) | ~60 |

---

## Electrònica

| Component | Quantitat | Observació |
|-----------|-----------|------------|
| BTT Octopus Pro V1.1 (H723) | 1 | Placa principal |
| BTT CB1 | 1 | Ordinador de control |
| TMC2209 (blau) | 3 | Drivers X, Y, extrusor |
| TMC5160-T Pro (vermell) | 2 | Drivers Z esq + Z dret |
| Font d'alimentació 24V | 1 | ≥15A recomanat |
| Llit calefactat 500×500mm | 4 | 24V (muntats en paral·lel cobrint 1000×1000mm) |
| CR Touch ALT04 | 1 | Sonda de nivellament |
| Final de carrera mecànic | 3 | X, Y, Z_max |

---

## Peces impreses en 3D

Vegeu [hardware/piezas-impresas.md](piezas-impresas.md) per al catàleg complet.

| Tipus | Color | Quantitat aproximada |
|-------|-------|---------------------|
| Suports motor NEMA 23 (X/Y) | Verd | 4 |
| Tensors corretja GT2 | Verd | 4-6 |
| Guies fusell (grises) | Gris | 4 |
| Portacables | Gris | ~20 |
| Suport endstop | Gris | 3 |

**Connectors de perfil metàl·lics** (no impresos): connectors angulars + femelles T per unir perfils item 40×80mm i ancorar guies lineals. Vegeu [fotos de connectors](../fotos/01-estructura/).

**Potes niveladoras amb bloqueig:** 4 unitats. Permeten moure i fixar l'estructura.

---

## Plànols estructurals

> Arxiu PDF del projecte (plànols del marc, mesures de tall de perfils, posicionament de components):

📎 [BTT_Octopus_pro_EN.pdf](../BTT_Octopus_pro_EN.pdf) — Manual de la placa base

> **Pendent**: Pujar el PDF de plànols estructurals del marc d'alumini (elaborat per l'equip del projecte).
