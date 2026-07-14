# Simulator fantomskih zastojev

Interaktivni simulator **fantomskih zastojev** (angl. *phantom traffic jams*) — zastojev, ki nastanejo brez vsakršne ovire, zgolj iz drobnih motenj v vožnji, ki se ojačajo v val, ki potuje nazaj po cesti.

Odprite [`index.html`](index.html) v brskalniku. Datoteka je samostojna (brez zunanjih odvisnosti).

## Parametri

Simulator ima štiri parametre, ki jih je zahtevala naloga:

| Parameter | Pomen v modelu |
|---|---|
| **Hitrost** | Največja / želena hitrost vozil (omejitev), v km/h. |
| **Verjetnost dogodka** | Verjetnost `p`, da voznik v posameznem koraku naključno zavre (nepozornost, previdnost). To je *jedro*, iz katerega nastane fantomski zastoj. |
| **Jakost zaviranja** | Koliko voznik pretirano zavre ob dogodku (jakost prereakcije). |
| **Gostota prometa** | Število vozil na kilometer. |

## Model

Uporabljen je model **Nagel–Schreckenberg** (1992) — celularni avtomat, v katerem je cesta razdeljena na celice (7,5 m), čas pa na korake (1 s). Vsako vozilo ima celoštevilsko hitrost `0…v_max` in v vsakem koraku sledi štirim pravilom:

1. **Pospešek** — `v → min(v+1, v_max)`
2. **Varnost** — `v → min(v, razmik do vozila spredaj)`
3. **Naključno zaviranje** — z verjetnostjo `p`: `v → max(v − jakost, 0)`
4. **Premik** — `pozicija → pozicija + v`

Vozila krožijo po zaprti zanki (obroč 3,0 km), enako kot v znamenitem Sugiyamovem poskusu. Ključna lastnost: pri `p = 0` (brez naključnosti) je tok popolnoma stabilen — fantomski zastoji nastanejo **samo** zaradi naključnih dogodkov, ne zaradi ovire.

### Vizualizacije

- **Krožna proga** — vozila kot točke, obarvana po hitrosti (rdeča = ustavljeno → zelena = prost tok).
- **Prostorsko-časovni diagram** — vsaka vrstica je en časovni korak; temne diagonale, ki potujejo v levo, so valovi zastoja, ki se pomikajo nazaj skozi kolono.
- **Meritve v živo** — povprečna hitrost, pretok (vozil/h), število zastojev, delež ustavljenih vozil.

## Študije v ozadju

- **Sugiyama et al. (2008)**, *Traffic jams without bottlenecks — experimental evidence*, New J. Phys. **10**, 033001. 22 vozil na 230 m krožni progi spontano tvori zastoj brez ovire. <https://iopscience.iop.org/article/10.1088/1367-2630/10/3/033001>
- **Nagel & Schreckenberg (1992)**, *A cellular automaton model for freeway traffic*, J. Phys. I France **2**, 2221. Osnovni model tega simulatorja. <https://hal.science/jpa-00246697/document>
- **Flynn, Kasimov, Nave, Rosales & Seibold (MIT)**, teorija »jamitonov« — fantomski zastoji kot potujoči valovi. <https://math.mit.edu/traffic/>

## Zagon

```
# katerikoli statični strežnik, npr.:
python3 -m http.server
# nato odprite http://localhost:8000
```

Ali preprosto dvokliknite `index.html`.
