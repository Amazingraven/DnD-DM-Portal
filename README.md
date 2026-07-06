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
