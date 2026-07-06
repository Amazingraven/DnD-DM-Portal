# Icespire Session Builder — Arbejdsgang & JSON-skema

Dette dokument beskriver, hvordan scripts omsættes til sessioner i **Icespire Session Builder**
(det lokale HTML-program). Det supplerer designsystem-prompten — designreglerne dér gælder stadig,
fordi builder-JSON i sidste ende renderes gennem samme designsystem.

> **Kilde til sandhed:** Skemaet herunder afspejler builderens nuværende kode. Når vi tilføjer en
> ny bloktype eller et felt til programmet, opdateres dette dokument i samme ombæring. Builderen er
> autoritativ — hvis kode og dokument er uenige, vinder koden.
>
> **Skema-version:** 3 (8 bloktyper, 5 temaer)
>
> **v3-ændring:** `divider` har nu et valgfrit `transition`-felt (`fade`/`slide`/`instant`), der styrer overgangseffekten for scenen efter dividerern under fremvisning.
>
> **v2-ændring:** `narrative` og `narrative-dm` har nu et valgfrit `skills`-array, så skill-badges vises i samme tekstboks som scenen. Den selvstændige `skills`-blok findes stadig (bagudkompatibel), men indlejring i scenen er den foretrukne måde.

---

## To output-tilstande

| Tilstand | Hvornår | Resultat |
|---|---|---|
| **Builder-JSON** (standard) | Almindelige sessioner: scener, DM-noter, skill checks, statblocks, HP | En `.json`-fil brugeren importerer i builderen og gør færdig |
| **Bespoke HTML** (fallback) | Sider der kræver specialwidgets builderen ikke har (alarm-tracker, Frontfordeling, puslespils-logik, Spotify-embed) | En komplet enkeltstående HTML-side efter designsystem-prompten |

Standard er **builder-JSON**. Falder kun tilbage til bespoke HTML, hvis et script hænger på en
mekanik der ikke kan udtrykkes som en eksisterende bloktype — eller hvis brugeren beder om at få
mekanikken tilføjet som en ny bloktype i programmet (foretrukket på sigt, da den så bliver genbrugelig).

---

## Arbejdsgang

1. Brugeren skriver et script (selv eller via ChatGPT).
2. Claude omsætter scriptet til **én `.json`-fil** efter skemaet herunder og afleverer den som fil.
3. Brugeren importerer filen i builderen (**Importér**-knappen) og gør sessionen færdig:
   omarrangerer, retter tekst, lægger billeder ind, vælger tema, fremviser.

Importér aldrig en færdig HTML-side — importen forstår **kun** JSON med et `blocks`-array.

---

## JSON-skema

En session er ét objekt:

```json
{
  "name": "Session-navn (vises i biblioteket)",
  "theme": "parchment",
  "blocks": [ /* liste af blok-objekter */ ]
}
```

- `name` — streng. Sessionens navn.
- `theme` — én af: `parchment`, `frost`, `woods`, `crypt`, `royal`. Gælder hele sessionen.
- `blocks` — array. Rækkefølgen i arrayet = rækkefølgen på siden.
- En `id` på selve sessionen er valgfri; builderen tildeler en ny ved import.

Hver blok er:

```json
{ "id": "unik-id", "type": "blok-type", "data": { /* felter for typen */ } }
```

- `id` — **påkrævet og unik inden for filen** (fx `b1`, `b2`, …). Bruges til rækkefølge,
  redigering og scroll-reveal. Claude genererer dem automatisk.
- `type` — én af de otte typer nedenfor.
- `data` — feltobjekt der matcher typen.

---

## Bloktyper

### `header` — sidens titel (svæver på baggrunden)
```json
{ "kicker": "DRAGONS OF ICESPIRE PEAK", "title": "Sessionsnavn", "mood": "Kort stemningslinje" }
```

### `image` — fuldbredde stemningsbillede
```json
{ "imageData": "", "caption": "Billedtekst" }
```
- `imageData` er en base64 data-URL. **Fra et script er den altid tom (`""`)** — den fungerer som
  pladsholder, og brugeren klikker billedet ind i builderen. Billeder kan ikke base64-indlejres via JSON på forhånd.

### `narrative` — flavor-tekst (spiller-synlig, ingen hemmeligheder)
```json
{ "heading": "Scene-overskrift", "text": "Fortællertekst. Linjeskift bevares (\n).",
  "skills": [ { "name": "PERCEPTION", "desc": "Valgfri narrativ beskrivelse" } ] }
```
- `skills` er valgfrit. Badges vises i samme boks som teksten, lige under fortællingen. Udelad eller sæt `[]` hvis scenen ingen skill checks har.

### `narrative-dm` — flavor-tekst med skjulte DM-noter
```json
{ "heading": "Scene-overskrift", "text": "Spiller-synlig fortælling.",
  "skills": [ { "name": "INSIGHT", "desc": "" } ],
  "dmNotes": "Skjult: DC-værdier, loot, dialog, taktik. Vises ALDRIG på spiller-TV." }
```
- `skills` er valgfrit og spiller-synligt; det vises mellem fortællingen og (den skjulte) DM-note. DC hører fortsat kun til i `dmNotes`.

### `skills` — selvstændig skill check-blok (spiller-synlig, uden DC)
```json
{ "items": [
  { "name": "INVESTIGATION", "desc": "Kort narrativ beskrivelse af hvad spilleren gør" },
  { "name": "PERCEPTION", "desc": "…" }
] }
```
- `name` = skill i UPPERCASE. `desc` = narrativ, ikke mekanisk (kan være tom). DC hører til i en `narrative-dm`-bloks `dmNotes`.
- **Foretrukket:** læg i stedet badges i `skills`-arrayet på `narrative`/`narrative-dm`, så de sidder i samme boks som scenen. Den selvstændige blok bevares til bagudkompatibilitet og til badges der ikke hører til en bestemt scene.

### `statblock` — modstander-stats (spiller-synlig, bruges i kamp)
```json
{ "title": "Modstander × 2", "hp": "13 (2d8+4)", "ac": "13", "speed": "30 ft.", "cr": "1/4",
  "str": 10, "dex": 14, "con": 15, "int": 6, "wis": 8, "cha": 5,
  "traits": "Fri tekst.", "attacks": "Shortsword: +4 to hit, 1d6+2 piercing." }
```
- `hp`, `ac`, `speed`, `cr`, `traits`, `attacks` er strenge. De seks abilities er tal (modifikatoren udregnes automatisk).

### `hptracker` — interaktiv HP-tracker (spiller-synlig)
```json
{ "combatants": [
  { "name": "Skelet #1", "max": 13, "cur": 13 },
  { "name": "Skelet #2", "max": 13, "cur": 13 }
] }
```
- `max` og `cur` er tal. Sættes typisk ens fra start.

### `divider` — sektionsskille
```json
{ "icon": "⚔", "transition": "fade" }
```
- Alternér gerne mellem `⚔` og `◆`.
- `transition` — én af: `fade` (blød indtoning, standard), `slide` (glider ind fra siden), `instant`
  (ingen animation). Styrer overgangseffekten for scenen, der starter lige efter dividerern, når
  man skifter til den under fremvisning (Næste/Forrige).

---

## Synlighedsregel (samme som designsystemet)

Spiller-synligt: `header`, `image`, `narrative` (+ heading/text), `skills` (navn + desc),
`statblock`, `hptracker`, `divider`.
Skjult: alt i `dmNotes` (DC-værdier, loot, dialog, taktik, instruktioner). Når der er hemmeligheder
til en scene, brug `narrative-dm` i stedet for `narrative`.

---

## Anbefalet script-struktur (gør konvertering pålidelig)

Ikke et krav, men jo tættere scriptet er på dette, jo renere bliver første udkast:

```
SESSION: <navn>     TEMA: <parchment|frost|woods|crypt|royal>

SCENE: <overskrift>
[FLAVOR] <fortællertekst>
[SKILLS] INVESTIGATION — <beskrivelse>; PERCEPTION — <beskrivelse>
[DM] <DC-værdier, loot, dialog, taktik>     ← gør scenen til en narrative-dm-blok
[STATBLOCK] <navn> | HP … | AC … | CR … | STR/DEX/CON/INT/WIS/CHA | Traits … | Attacks …
[HP] <navn> max/cur; <navn> max/cur
[IMAGE] <billedtekst>     ← bliver en tom pladsholder-blok
[DIVIDER]
```

---

## Minimal komplet eksempel

```json
{
  "name": "Session 7 — Krypten under Axeholm",
  "theme": "crypt",
  "blocks": [
    { "id": "b1", "type": "header", "data": {
      "kicker": "DRAGONS OF ICESPIRE PEAK",
      "title": "Krypten under Axeholm",
      "mood": "Kold stentavshed · Noget rører sig i mørket" } },
    { "id": "b2", "type": "image", "data": { "imageData": "", "caption": "Indgangen til krypten" } },
    { "id": "b3", "type": "narrative", "data": {
      "heading": "Den Sorte Metaldør",
      "text": "Døren er kold mod håndfladen. Runerne langs kanten gløder svagt." } },
    { "id": "b4", "type": "skills", "data": { "items": [
      { "name": "INVESTIGATION", "desc": "Undersøg dørens mekanisme" },
      { "name": "PERCEPTION", "desc": "Lyt efter lyde bag døren" } ] } },
    { "id": "b5", "type": "narrative-dm", "data": {
      "heading": "Blodkammeret",
      "text": "Rummet åbner sig om jer, og luften smager af jern.",
      "dmNotes": "Investigation DC 15: skjult niche med ruby-fragment. Loot: 40 gp, Potion of Healing." } },
    { "id": "b6", "type": "statblock", "data": {
      "title": "Skeletvogter × 2", "hp": "13 (2d8+4)", "ac": "13", "speed": "30 ft.", "cr": "1/4",
      "str": 10, "dex": 14, "con": 15, "int": 6, "wis": 8, "cha": 5,
      "traits": "Undead. Vulnerability: bludgeoning.",
      "attacks": "Shortsword: +4 to hit, 1d6+2 piercing." } },
    { "id": "b7", "type": "hptracker", "data": { "combatants": [
      { "name": "Skelet #1", "max": 13, "cur": 13 },
      { "name": "Skelet #2", "max": 13, "cur": 13 } ] } },
    { "id": "b8", "type": "divider", "data": { "icon": "◆" } }
  ]
}
```
