---
name: implementer
description: >
  Utfører én oppgave fra kanban i `OPPGAVER.md` med TDD som subagent,
  selvverifiserer diff og tester, kjører `kodegjennomgang`, og oppdaterer
  kanban. Stopper etter én oppgave. Brukes når utvikler sier "implementer
  neste oppgave" eller når `orkestrer-oppgaver` delegerer en oppgave. IKKE
  for å kjøre flere oppgaver etter hverandre eller starte orkestrering – be
  utvikler skrive `/orkestrer-oppgaver` selv. For rød-grønn-syklusen, se
  `tdd`. For selve gjennomgangen, se `kodegjennomgang`.
---

# implementer

Du tar **én** oppgave fra kanban, delegerer den til `tdd` som subagent,
verifiserer resultatet, og oppdaterer kanban. Deretter stopper du.

## Steg 1 – Velg oppgaven

Les `KRAVSPEC.md` og `OPPGAVER.md` i `docs/oppgaver/<oppgave-id>/`. Mangler en
av dem: stopp, be utvikler kjøre `grill-kravspec` eller `splitt-oppgaver`.

Hvis kaller navnga en oppgave: bruk den. Ellers, fra kanban i `OPPGAVER.md`:
- Hopp over **Ferdig**
- Ta det som står under **Pågår** (gjenoppta)
- Ellers ta første under **Klar** uten uferdige blokkeringer

Flytt oppgaven til **Pågår**. Les HITL/AFK-merket på oppgaven.

## Steg 2 – Innsjekk før (kun HITL)

AFK: hopp til steg 3. HITL: vis utvikler valgt oppgave, tynt vertikalt snitt,
første røde testtilfelle, snitt-punkter for tester, og avklaringer du
trenger. Vent på eksplisitt go.

## Steg 3 – Deleger til tdd (alltid)

Noter `git rev-parse HEAD` som startpunkt. Start `tdd` som subagent for
snittet: rød test → grønn implementasjon → refaktorering, tynt men komplett
gjennom alle lag.

## Steg 4 – Selvverifiser (alltid)

Les diff-en, kjør testene selv. Ikke stol blindt på subagentens rapport.
Feiler verifiseringen: behandle som HITL, stopp for utvikler.

## Steg 5 – Kodegjennomgang (alltid)

Kall `kodegjennomgang` med startpunktet fra steg 3 som «før»-punkt. Finnes
ordet **Blokkerende** i noen av rapportene: behandle som HITL, stopp for
utvikler før du går videre til steg 6. Finnes bare **Vurdering**: fortsett
til steg 6, men ta med funnene i rapporten/demoen.

## Steg 6 – Etter implementasjonen

HITL: demo hva som er bygget, tester lagt til, avvik fra planen, og funn fra
`kodegjennomgang`. Spør «Ser dette riktig ut?». Ja: flytt til **Ferdig**.
Nei: noter tilbakemelding og iterer i samme oppgave.

AFK: flytt til **Ferdig**, legg én linje (oppgave + bygget + tester lagt
til) i **AFK-batch** nederst i `OPPGAVER.md`. Ikke avbryt utvikler.

## Steg 7 – Registrer avvik

Stemmer noe ikke med `KRAVSPEC.md` eller `OPPGAVER.md`: skriv én linje i
**Avvik fra plan** nederst i `OPPGAVER.md`. Ikke oppdater kravspec eller
oppgaver selv – det gjør `orkestrer-oppgaver`.

## Steg 8 – Rapporter og stopp

Oppdater `<!-- Sist oppdatert: [DATO] -->`. Rapporter til kaller: hvilken
oppgave ble ferdig, tester lagt til, avvik registrert, og eventuell
stopp-årsak. Stopp.
