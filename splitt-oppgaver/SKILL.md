---
name: splitt-oppgaver
description: Splitter en ferdig kravspesifikasjon i tracerkule-oppgaver og organiserer dem som kanban.
disable-model-invocation: true
---

# splitt-oppgaver

Du bryter ned en kravspesifikasjon i små, uavhengige oppgaver. Hver oppgave er en
**tracerkule** – et tynt vertikalt snitt som går gjennom alle lag (DB, API, UI, test)
og kan demonstreres alene.

## Steg 1 – Hent inn grunnlaget

Be utvikler peke på oppgavemappen `docs/oppgaver/<oppgave-id>/` (fra `grill-kravspec`).
Les hele `KRAVSPEC.md`. Utforsk berørt kode hvis du ikke allerede har gjort det –
bruk samme begrepsbruk som kravspecen.

## Steg 2 – Tegn opp vertikale snitt

Bryt arbeidet i tracerkule-oppgaver etter disse reglene:

- Hvert snitt leverer en smal, men **komplett** vei gjennom alle lag
- Et ferdig snitt skal kunne demoes eller verifiseres alene
- Foretrekk mange tynne snitt fremfor få tykke
- Marker hver oppgave som **HITL** (krever menneskelig avgjørelse / review) eller
  **AFK** (kan implementeres uten avbrudd). Foretrekk AFK.

## Steg 3 – Kvalitetssikre med utvikler

Vis utkastet som en nummerert liste. For hver oppgave:

- **Tittel**
- **Type**: HITL / AFK
- **Blokkert av**: andre oppgaver (om noen)
- **Dekker**: hvilke brukerhistorier fra kravspecen

Kall deretter `grilling`. Tema: er granulariteten riktig (for grov / for
fin), stemmer avhengighetene, bør noen oppgaver slås sammen eller deles
ytterligere, er HITL/AFK riktig markert. Iterer til utvikler godkjenner.

## Steg 4 – Skriv kanban-fil

Lagre som `docs/oppgaver/<oppgave-id>/OPPGAVER.md` med mal fra
[OPPGAVER-MAL.md](OPPGAVER-MAL.md). Bruk samme `<oppgave-id>` som kravspecen.

Inkluder:
- Lenke til `KRAVSPEC.md` (relativ – samme mappe)
- Kanban-kolonner: **Klar** / **Pågår** / **Ferdig**
- Et mermaid-diagram som viser avhengigheter mellom oppgaver
- Sett inn dagens dato i `<!-- Generert av KI med menneskelig supervensjon. Sist oppdatert: [DATO] -->`

## Steg 5 – Idempotens

Hvis filen finnes fra før: les den, behold status (Pågår/Ferdig) på eksisterende
oppgaver, og merge inn nye uten å overskrive arbeid som er gjort.
