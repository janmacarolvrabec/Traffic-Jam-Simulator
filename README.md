# Simulator fantomskih zastojev

Interaktivni simulator **fantomskih zastojev** (angl. *phantom traffic jams*) — zastojev, ki nastanejo brez vsakršne ovire, zgolj iz drobnih motenj v vožnji, ki se ojačajo v val, ki potuje nazaj po cesti.

Poskus pripravil **Jan Macarol**.

Simulator ponuja **dva poskusa**:

1. **Krožna proga** — vozila krožijo v zaprti zanki (à la Sugiyama, 2008). Fantomski zastoji nastanejo spontano iz naključnih mikro-zaviranj. Nastavljivi parametri: hitrost, verjetnost dogodka, jakost zaviranja, gostota.
2. **En avto zavre** — raven kilometer ceste **brez naključnih motenj**. S klikom ustavite eno vozilo in opazujete, kako za njim nastane trajna kolona, ki potuje nazaj po cesti — tudi ko vaše vozilo že odpelje naprej.
3. **110 proti 130** — A/B primerjava dveh enakih cest z isto gostoto, kjer se spremeni le omejitev hitrosti. Verjetnost dogodka **raste s hitrostjo** (`p ∝ v`): v enakem reakcijskem času (~1 s) pri 130 km/h prevozite več metrov, zato je manj rezerve in več dogodkov. Simulator sproti meri **potovalno hitrost, delež časa v koloni in dogodke na 100 km** za obe omejitvi.

Privzete vrednosti so umerjene na **realni slovenski avtocestni promet** (omejitev 130 km/h, ~10 % nepozornost, gost promet 30 voz/km → zmogljivost pasu ~2400 voz/h, kar ustreza literaturi 2000–2400 voz/h/pas).

### Ugotovitev Poskusa 3 (110 vs 130)

Iz izmerjenih podatkov modela (z verjetnostjo dogodka, odvisno od hitrosti, in počasnim speljevanjem, ki ustreza zlomu pretoka na avtocestah): pri gostem prometu je **potovalna hitrost pri 110 in 130 praktično enaka**, a pri 130 km/h voznik preživi **več časa v koloni (stop-and-go)** in doživi **več dogodkov na 100 km**. Ker višja omejitev v gneči ne prihrani časa, a poveča tveganje in nemirnost prometa, je **nižja omejitev smiselna**. To je ista logika, na kateri temeljijo **spremenljive omejitve hitrosti** na avtocestah.

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
- **Temeljni diagram (pretok–gostota)** — krivulja za trenutne parametre z označeno *kritično gostoto* (vrhom zmogljivosti) in točko trenutnega stanja v živo. Pokaže, da čez kritično gostoto pretok pade, čeprav vozil dodajamo.
- **Meritve v živo** — povprečna hitrost, pretok (vozil/h), število zastojev, delež ustavljenih vozil.

## Deljenje na družbenih omrežjih

Datoteka vsebuje oznake **Open Graph** in **Twitter Card** ter gumbe za deljenje na Facebook, X, LinkedIn in WhatsApp. Priložena je predogledna slika `og-image.png` (1200×630).

Za lepe predoglede (naslov + slika) pri deljenju objavite stran na spletu, npr. prek **GitHub Pages**: v nastavitvah repozitorija vklopite Pages z veje, nato bo dostopna na `https://<uporabnik>.github.io/Traffic-Jam-Simulator/`. Oznake `og:url` / `og:image` v `index.html` že kažejo na ta naslov (po potrebi prilagodite uporabniško ime).

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
