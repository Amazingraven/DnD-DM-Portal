# DnD-DM-Portal — DM-admin (privat)

**Privat repo.** Indeholder DM-værktøjet (builderen) og projektdokumentation. Ingen faktiske
sessionsdata ligger her, medmindre du bevidst lægger en backup-eksport i `backups/`.

## Indhold

- `index.html` — Icespire Session Builder. Hostes via GitHub Pages på en fast adresse, så den
  ikke er bundet til én lokal fil/browser-oprindelse.
- `dnd-session-projekt-prompt.md` — designsystem og bespoke-HTML-fallback.
- `icespire_builder_workflow.md` — JSON-skema for builder-import (bloktyper, temaer).
- `backups/` — periodiske eksporter af dit sessionsbibliotek (via builderens backup-funktion).
  Bruges også til manuel sync mellem flere pc'er: eksportér på maskine A, læg filen her, hent og
  importér på maskine B.
- `lobby/index.html` — opstarts-skærmen (Ken Burns-slideshow med quest-info og nedtælling), åbnes
  via "🕯 Opstarts-skærm"-knappen i builderens topbar. Tilgås direkte på
  `https://amazingraven.github.io/DnD-DM-Portal/lobby/`.
- `shops/index.html` — fuld inventory-styring for butikkerne (køb/salg/redigering/tilføj/slet).
  **Kun til dig** — data ligger i browserens localStorage, synkroniserer ikke automatisk med
  spillerportalen. Brug "⬇ Eksportér til spillerportal"-knappen og send filen til Claude for at
  opdatere den read-only `shops.json` i `DnD-Portal`, som spillerne ser.
- `content/index.html` — Portal Content Manager: form-baseret editor til NPC'er, items, podcasts
  og session-logs (de fire andre datafiler i `DnD-Portal/data/`). Faner for hver type, "+ Tilføj",
  redigér/slet pr. post. **"🔄 Hent nyeste fra portal"** henter det, der allerede ligger live, ind i
  editoren, så du ikke starter forfra. **"⬇ Eksportér JSON"** downloader den redigerede fil —
  send den til Claude, som pusher den til `DnD-Portal`.

## Vigtigt

- Dette repo er **privat** — hold det sådan, da builderen (og evt. backups) kan indeholde
  kampagnehemmeligheder og DM-noter.
- Din faktiske sessions-data ligger som udgangspunkt lokalt i browserens IndexedDB, ikke her.
  Kun hvis du selv eksporterer og lægger filen i `backups/`, havner data i git.
- Builderens kode er autoritativ kilde til JSON-skemaet — ændres en bloktype i `index.html`,
  opdateres `icespire_builder_workflow.md` i samme ombæring.

## Live adresse

`https://amazingraven.github.io/DnD-DM-Portal/` (kræver at GitHub Pages er aktiveret under
repo → Settings → Pages → Source: branch `main`, mappe `/root`).
