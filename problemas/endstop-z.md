# Problema: Endstop Z de seguretat — Configuració incorrecta

## Descripció del problema

Necessitàvem un endstop de seguretat per a l'eix Z màxim — per evitar que el capçal pugi indefinidament i xoqui contra el marc. La impressora té 1000mm de recorregut Z i sense un límit superior el motor podria destruir mecànica.

El connector T3 de la placa (pin `PF7`) es va reutilitzar com a entrada digital per a aquest fi, ja que no s'estava usant per a la seva funció original.

## Causa del problema inicial

La configuració de `endstop_pin` amb `^PF7` (pull-up) no funcionava correctament. El pin T3 en la placa requereix un pull-**down** en lloc d'un pull-up, degut a la manera com està dissenyada la resistència interna.

## Solució

Usar `~PF7` (virgulilla = pull-down actiu baix) en lloc de `^PF7` (circumflex = pull-up actiu baix):

```ini
[stepper_z]
endstop_pin: ~PF7   # ~ = pull-down (T3 connector)
position_max: 1010
position_min: -5
```

Per a la seguretat addicional, es va configurar una aturada d'emergència via `[gcode_button]`:

```ini
[gcode_button endstop_z_max]
pin: ~PF7
press_gcode:
    M112   # aturada d'emergència si s'activa el final de carrera superior
```

## Nota sobre els símbols en Klipper

| Símbol | Significat |
|--------|------------|
| `^` | Pull-up intern activat (actiu baix) |
| `~` | Pull-down intern activat (actiu alt) |
| `!` | Invertir lògica del pin |

## Resultat

El final de carrera Z màxim funciona correctament. Si el capçal arriba al límit superior, Klipper atura tots els moviments immediatament.

## Lliçó

- No tots els pins es comporten igual — T3 (`PF7`) necessita pull-down (`~`), no pull-up (`^`)
- Sempre verificar el comportament elèctric del pin abans de connectar i configurar
- Per a final de carrera de seguretat crític, afegir `M112` via `[gcode_button]`
