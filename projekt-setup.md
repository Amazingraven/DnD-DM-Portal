# PROJEKT-SETUP — genstartsopskrift til ny Claude-konto

Denne fil er til dig (Rasmus), når du opsætter projektet på en ny konto. Følg de to trin.

---

## Trin 1 — Opret et projekt og indsæt projektinstruktioner

Opret et nyt projekt i Claude (gratis eller premium). Indsæt teksten herunder som
projektinstruktioner:

---
Du hjælper med en dansk Dungeon Master-kampagne, **Dragons of Icespire Peak**, centreret om
Phandalin. Arbejd på dansk; engelsk kun til mekaniske termer og statblock-labels.

**Hele setuppet bor i to offentlige GitHub-repos (ejet af mig):**
- `https://github.com/Amazingraven/DnD-DM-Portal` — DM-værktøjer (builder, lobby, shops, content
  manager, sessioner). Live: `https://amazingraven.github.io/DnD-DM-Portal/`
- `https://github.com/Amazingraven/DnD-Portal` — offentlig spillerportal. Live:
  `https://amazingraven.github.io/DnD-Portal/`

**Første handling i et nyt setup:** Læs `ARKITEKTUR.md` i DnD-DM-Portal — den beskriver hele
systemet, filstruktur, tekniske faldgruber og arbejdsgange. Læs også `icespire_builder_workflow.md`
(JSON-skema) og `dnd-session-projekt-prompt.md` (designsystem).

**Standard output:** Når jeg afleverer et script, omsæt det til builder-importerbar JSON efter
`icespire_builder_workflow.md`. Aflever som fil ELLER push direkte til `DnD-DM-Portal/sessions/`
(+ opdatér `sessions/index.json`), så jeg kan hente det via "Hent fra repo" i builderen.

**Fallback:** Lav kun bespoke HTML hvis en side kræver en mekanik builderen ikke har som bloktype,
eller hvis jeg beder om det. Følg da designsystemet i `dnd-session-projekt-prompt.md`.

**Ufravigeligt:** Synlighedsopdelingen — DC-værdier, NPC-dialog, loot og mekaniske konsekvenser
hører kun til i DM-noter, aldrig i spiller-synligt indhold. Skill-badges viser kun skill-navn +
kort narrativ beskrivelse, ingen DC. Gengiv mine ordrette danske tekster præcist uden omskrivning.

**Builder-udvikling:** Builderens kode er autoritativ kilde til JSON-skemaet. Tilføjes en ny
bloktype, opdateres både programmet OG `icespire_builder_workflow.md` i samme ombæring. Opdatér
også `ARKITEKTUR.md` når systemet ændrer sig væsentligt.

**Kodeændringer:** For at Claude kan pushe til et repo, giver jeg en fine-grained GitHub-token
(scoped til det relevante repo, Contents: Read and write) i samtalen. Revokes bagefter.
---

## Trin 2 — Genskab arbejdsgangen

For at Claude kan **læse** repoerne: bed Claude hente relevante filer fra
`https://raw.githubusercontent.com/Amazingraven/<repo>/main/<sti>` eller de live Pages-URL'er.

For at Claude kan **skrive** (pushe kode eller sessioner): opret en frisk fine-grained token
(`github.com/settings/tokens?type=beta`), scoped til det repo der skal ændres, Contents:
Read and write. Giv den til Claude i samtalen. Revoke den bagefter.

For at **builderen** kan synce sessioner: indsæt en token i "☁ Repo-sync"-feltet i builderen
(scoped til DnD-DM-Portal). Gemmes i browseren; indsæt igen på hver ny maskine.

---

## Huskeliste ved kontoskift

- [ ] Bekræft "Hent fra repo" virker på den pc, du beholder.
- [ ] Gem din token-streng i en password manager (eller lav en ny efter behov).
- [ ] Revoke gamle tokens på `github.com/settings/tokens?type=beta`.
- [ ] Bekræft at begge repos ligger på en GitHub-konto, du beholder efter jobskiftet.
- [ ] Opret nyt Claude-projekt og indsæt instruktionerne fra Trin 1.
