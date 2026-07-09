# ARKITEKTUR — Dragons of Icespire Peak toolchain

> **Til en frisk Claude / ny konto:** Læs dette dokument først. Det beskriver hele systemet, så du
> kan arbejde videre uden adgang til tidligere samtaler. Kode i repoerne er den autoritative kilde;
> hvis dette dokument og koden er uenige, vinder koden — og så bør dokumentet opdateres.

Kampagnen er **Dragons of Icespire Peak** (Phandalin). Alt spiller-/narrativt indhold er på **dansk**;
kun mekaniske termer og statblock-labels er engelske.

---

## To repos (begge offentlige, hostet via GitHub Pages)

### 1. `DnD-DM-Portal` — privat DM-værktøjskasse (dette repo)
Live: `https://amazingraven.github.io/DnD-DM-Portal/`

| Sti | Hvad |
|---|---|
| `index.html` | **Icespire Session Builder** — hovedværktøjet. Bygger/fremviser sessioner. |
| `lobby/index.html` | Opstarts-skærm til TV (Ken Burns-slideshow + nedtælling). |
| `shops/index.html` | Butiks-/inventory-styring (fuld redigering, localStorage). |
| `content/index.html` | Portal Content Manager — form-editor til spillerportalens data. |
| `sessions/` | Sessioner pushet fra builderen via repo-sync. `index.json` = liste. |
| `backups/` | Manuelle bibliotekseksporter (valgfrit). |
| `icespire_builder_workflow.md` | JSON-skema for sessioner (bloktyper, temaer). |
| `dnd-session-projekt-prompt.md` | Designsystem + bespoke-HTML-fallback. |

### 2. `DnD-Portal` — offentlig spillerportal
Live: `https://amazingraven.github.io/DnD-Portal/`

| Sti | Hvad |
|---|---|
| `index.html` | Spiller-side: faner for Session-logs, Podcasts, NPC'er, Fund, Butikker. |
| `data/npcs.json` | "Folk vi har mødt". |
| `data/items.json` | "Fund & genstande". |
| `data/podcasts.json` | Links til session-optagelser. |
| `data/sessions.json` | Session-recaps (spoiler-fri). |
| `data/shops.json` | **Read-only** butiksvisning (spejles fra `shops/`-værktøjet). |

**Synlighedsprincip (ufravigeligt):** DC-værdier, NPC-dialog, loot og mekaniske konsekvenser hører
KUN til i DM-noter (`dmNotes` / `narrative-dm`). Spillerportalen må aldrig indeholde spoilere.

---

## Session Builder — nøglefunktioner (`index.html`)

- **8 bloktyper:** header, image, narrative, narrative-dm, skills, statblock, hptracker, divider.
- **5 temaer:** parchment, frost, woods, crypt, royal.
- **Lagring:** IndexedDB lokalt (`icespire_db`), fallback til localStorage.
- **Fremvis til TV:** åbner et separat popup-vindue. Dividers deler sessionen i "sider";
  Næste/Forrige (topbar-knapper, pil-taster, mellemrum) skifter side.
- **Skærmvalg:** "🖥 Vælg skærm" bruger Window Management API (Chrome/Edge) til at åbne
  fremvisningen på en valgt skærm i fuldskærm.
- **HP-tracker:** +/− knapper OG et input-felt der virker som lommeregner (skriv `-8`, `+5`,
  `15-8+2`). Intet øvre loft (kan overstige max, fx temp HP); bund = 0.
- **Divider-overgang:** hver divider har `transition` = fade | slide | instant.
- **Repo-sync ("☁ Repo-sync" i sidepanelet):** to-vejs sync af sessioner med `sessions/`.
  Token gemmes i browserens localStorage (`icespire-gh-token`). "Gem session i repo" pusher;
  "Hent fra repo" loader. Se token-afsnit nedenfor.

---

## Token-opsætning (repo-sync)

Builderen skriver til GitHub via en **fine-grained personal access token**, gemt i browseren:
- Scope: **kun** `DnD-DM-Portal`, **Contents: Read and write**.
- Gemmes pr. browser/maskine (følger ikke med på tværs af pc'er — indsæt igen hver maskine).
- Opret på `github.com/settings/tokens?type=beta`.

For at **Claude** kan pushe kodeændringer (i en samtale) skal brugeren give en tilsvarende token
i samtalen. Den bør revokes bagefter og laves frisk næste gang.

---

## Vigtige tekniske faldgruber (lært undervejs — undgå at genopfinde)

1. **`document.write` + `document.close()` dropper inline `<script>`.** Fremvisningsvinduets
   funktioner (`__showPage`, `__step`, HP) må IKKE ligge i et script skrevet ind i popup'en —
   de defineres fra hovedappen på popup-vinduets `window` via `installPresentControls(win, doc)`.
2. **Paginering finder blokke via `querySelectorAll('#player-root .pblock')`**, ikke via
   `player-root.children` — der er en `.page-wrap`-indpakning imellem.
3. **UTF-8 (æøå) i GitHub Contents API:** base64 skal laves med
   `btoa(unescape(encodeURIComponent(str)))`, ellers ødelægges danske tegn.
4. **Contents API PUT kræver filens nuværende `sha`** ved opdatering (hent den først), ellers 409.
5. **GitHub Pages på private repos kræver betalt plan** — derfor er begge repos offentlige.
6. **Fine-grained tokens kan ikke aktivere Pages via API** (mangler Pages-perm) — slå Pages til
   manuelt i repo-indstillinger første gang.
7. **HTML kollapser whitespace:** brug `escNL()` (newline→`<br>`) for flerlinje-felter som
   statblock Traits/Attacks.
8. **4K/fullscreen:** IntersectionObserver scroll-reveal droppet (dårlig ydelse); Ken Burns bruger
   GPU-kompositerede transforms, peak-zoom ≤1.16×; billeder gemmes ved 3840px/kvalitet 0.92.

---

## Arbejdsgang for nyt session-indhold

1. Bruger skriver/genererer et script (ChatGPT eller selv).
2. Claude konverterer til builder-JSON efter `icespire_builder_workflow.md`.
3. **Enten:** Claude pusher JSON direkte til `sessions/` (+ opdaterer `sessions/index.json`),
   og brugeren klikker "Hent fra repo" — **eller** brugeren importerer en JSON-fil manuelt.
4. Bruger færdiggør i builderen: billeder, tema, rækkefølge, fremvisning.

## Arbejdsgang for spillerportal-indhold

1. Brug `content/`-editoren (eller rediger `DnD-Portal/data/*.json` direkte).
2. Eksportér JSON derfra, eller bed Claude pushe til `DnD-Portal`.
3. Shops: rediger i `shops/`-værktøjet, eksportér, opdatér `DnD-Portal/data/shops.json`.
