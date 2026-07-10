# STIL — sådan skrives sessioner til denne kampagne

> Til Claude: Denne guide er destilleret fra to faktiske sessioner (begge i `reference/` og
> `sessions/`): **"The Killing Fields of Axeholm"** (kamptung, bølgebaseret) og
> **Axeholm-dungeon** (udforsknings-/lore-tung). Standardformat er **builder-JSON** efter
> `icespire_builder_workflow.md`. Denne guide handler om *tone, struktur og indhold* — ikke skema.

---

## Grundtone

- **Sprog:** Dansk narrativ og DM-tekst. Engelske scene-overskrifter (fx "The Great Hall",
  "Death Fog in the Corridors"). Engelske mekaniske termer og statblock-labels; danske
  oversættelser af traits/attacks-tekst (fx "Ramt: 9 (2d6+2) stik").
- **Stemning:** Stemningsfuld, sanselig, lidt gotisk. Kulde, tåge, støv, "kald fra det hinsides".
  Fortællerteksten maler et rum, før den giver spillerne noget at handle på.
- **Header:** kicker med kapitelnummer (`DRAGONS OF ICESPIRE PEAK · KAPITEL 11`), kort engelsk
  titel, ofte tom mood.

## Sceneopbygning

Hver scene er typisk: **billede → narrative/narrative-dm → skills → (ved kamp: statblocks +
hptracker)**, adskilt af **dividers**. Billedtekster er fortællende, ikke tekniske
("Vejen til Axeholm — tågen er væk, men de døde er ikke alene om at bemærke det").

- **Spiller-synlig narrative:** engelsk overskrift, dansk brødtekst, 1-3 skill-badges **uden**
  beskrivelse (kun skill-navnet — spillerne tolker selv relevansen).
- **Skjult i `dmNotes`:** ALT mekanisk og hemmeligt. Se nedenfor.

## `dmNotes` er DM-coaching, ikke bare tal — dette er signaturtrækket

Dine DM-noter forklarer *hvorfor*, ikke kun *hvad*. De indeholder typisk:
- **Sammensætning + rå XP** ("4 Ghouls + 3 Zombies. ≈1200 XP rå").
- **Trigger** (hvornår encounteret starter).
- **Formål** ("Opvarmning. Etablér straks, at nogle udøde ignorerer heltene…").
- **Hvordan monstre spilles** ("Wraithen ER den størknede dødståge: flyv gennem vognene, gå efter
  de svage").
- **Konkrete alternativer** ("ALTERNATIV — byt Wraith + Specters ud med Wight »Tågeslugeren«…").
- **Tuning for gruppen** (fast reference: **stærk gruppe på seks**; faren er *action economy*,
  ikke CR; **objektiv-tryk slår rå HP**; "ingen hvile mellem bølger").
- **Outcome** (hvad der sker bagefter; ofte "pusterum-beat", valgfri narrative echo).

## Kampstruktur

- Byg kamp i **bølger**, adskilt af dividers, med et billede + narrative-dm som optakt til hver.
- **Statblocks:** engelske labels, `×`-antal i titlen ("Ghoul × 4"), danske trait/attack-tekster.
  Brug linjeskift i traits/attacks (renderes som separate linjer).
- **HP-tracker:** én række per fjende, individuelt nummereret ("Ghoul #1"… "Zombie #3").
- Afslut gerne med en **"Til Spillerne"**-narrative-dm: brødtekst = et stemningsfuldt valg til
  bordet; `dmNotes` = din tuning-filosofi og outcome.

## Udforsknings-/lore-sessioner

- Tungere på **key-info** (mange små nøgleoplysninger) og rollespil ("fungerer mere som
  rollespilsrum end stramt script — giv NPC'en plads til at styre tempoet").
- Gated afsløringer: inskriptioner, breve, lore bag skill-checks ("The Letter (hvis
  investigation)"; "Investigation (elvisk): »Her hviler Aelarion Vael«").
- Foreshadowing og lag af historie ("Krypten antyder, at Phandalin er bygget oven på ældre lag";
  hooks til senere questlines).

## Synlighedsprincippet (ufravigeligt)

Spillerne ser: fortællertekst, skill-badge-navne, statblocks, HP-tracker, billeder.
Skjult i `dmNotes`: DC-værdier, NPC-dialog der ikke er sagt endnu, loot, taktik, XP, triggers,
alternativer, outcome. Når en scene har hemmeligheder → brug `narrative-dm`, ikke `narrative`.

## Navngivning & verden

Faste elementer: Phandalin, Axeholm, Cryovain (hvid drage), Neverwinter Woods.
NPC'er: Halia, Garaele, Big Al, Dazlyn, Norbus, Tolbin, Elyra Alagondar m.fl.
Tema for Axeholm-materiale: `crypt`.
