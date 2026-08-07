---
name: implementer
description: >
  Utfører én oppgave fra kanban i `OPPGAVER.md` med TDD som subagent,
  selvverifiserer diff og tester, og oppdaterer kanban + markør i
  `ORKESTRATOR.md`. Stopper etter én oppgave. Brukes når utvikler sier
  "implementer neste oppgave", "kjør én oppgave", eller når
  `orkestrer-oppgaver` delegerer en oppgave. For løkke over flere oppgaver,
  se `orkestrer-oppgaver`. For selve rød-grønn-syklusen, se `tdd`.
---

# implementer

Du tar **én** oppgave fra kanban-brettet, delegerer den til `tdd` som
subagent, verifiserer at koden gjør det den sier, og oppdaterer kanban og
markør. Så stopper du. Løkken over flere oppgaver eies av `orkestrer-oppgaver`.

## Hvem eier hva

- **Kanban i `OPPGAVER.md` eier status.** Klar / Pågår / Ferdig er eneste
  sannhet. Ingen andre filer speiler status.
- **`ORKESTRATOR.md` eier markør, logg og batch.** Du oppdaterer den, men
  gjentar ikke kanban der.

## HITL vs AFK

Hver oppgave er merket **HITL** eller **AFK** i `OPPGAVER.md`. Merket styrer
menneskelig involvering:

- **HITL**: full innsjekk før (steg 3) og demo/godkjenning etter (steg 5).
- **AFK**: hopp over forhåndsinnsjekken, kjør oppgaven, samle resultatet i
  batch i `ORKESTRATOR.md`.

Selvverifiseringen i steg 4 kjører **alltid**. Et nytt avvik (steg 6) er
alltid grunn til å stoppe.

## Steg 1 – Finn grunnlaget

Be utvikler peke på oppgavemappen `docs/oppgaver/<oppgave-id>/`. Les:
- `KRAVSPEC.md` – kontekst og begreper
- `OPPGAVER.md` – kanban og HITL/AFK-merke
- `ORKESTRATOR.md` – markør, batch, avvik

Hvis noen mangler: stopp og be utvikler kjøre riktig skill først
(`grill-kravspec`, `splitt-oppgaver`).

## Steg 2 – Velg oppgaven

Hvis kaller (utvikler eller `orkestrer-oppgaver`) navnga en oppgave: bruk den.
Ellers:

- Hopp over alt under **Ferdig**
- Hvis noe står under **Pågår**: ta den (gjenoppta)
- Ellers: ta første under **Klar** hvor alle blokkerende oppgaver er ferdige

Flytt oppgaven til **Pågår** i kanban og pek markøren i `ORKESTRATOR.md` på
denne oppgaven. Les HITL/AFK-merket.

## Steg 3 – Innsjekk før (kun HITL)

**AFK:** hopp rett til steg 4.

**HITL:** vis utvikler før implementasjonen starter:
- Hvilken oppgave du tar
- Hvordan du planlegger å snitte den (tynt vertikalt – DB → API → UI → test)
- Hva som blir det første røde testtilfellet
- Hvilke snitt-punkter tester skal skrives mot
- Hvilke avklaringer du trenger (om noen)

Vent på eksplisitt go.

## Steg 4 – Deleger til tdd-subagent (alltid)

Start en subagent som bruker `tdd`-skillen til å implementere snittet:
rød test → grønn implementasjon → refaktorering. Hold snittet tynt men
komplett gjennom alle lag.

Når subagenten er ferdig: **verifiser** at koden faktisk gjør det den sier.
Les diff-en, kjør tester. Ikke stol blindt på rapporten. Dette gjelder
uansett HITL/AFK.

Hvis selvverifiseringen feiler: behandle som HITL, stopp for utvikler.

## Steg 5 – Etter implementasjonen

**HITL:** demo for utvikler – hva som er bygget (kort demo eller eksempel),
hvilke tester som ble lagt til, eventuelle avvik fra planen. Spør:
«Ser dette riktig ut før vi går videre?» Hvis ja: flytt oppgaven til
**Ferdig** i kanban og oppdater markøren. Hvis nei: noter tilbakemelding og
iterer i samme oppgave.

**AFK:** flytt oppgaven til **Ferdig** i kanban, oppdater markøren, og legg
én linje (oppgave + hva som ble bygget + tester lagt til) i **AFK-batch** i
`ORKESTRATOR.md`. Ikke avbryt utvikler.

## Steg 6 – Kode-gjennomgang

Kall `kode-gjennomgang`-skillen mot det faste punktet (f.eks. branchen oppgaven
ble implementert på). Den kjører Standard- og Spec-aksen i parallell og
rapporterer funnene. Blokkerende funn (krav som mangler, klare standardbrudd):
noter i **Avvik fra plan** og behandle som HITL — stopp for utvikler.

## Steg 7 – Registrer avvik

Hvis du oppdaget noe som ikke stemmer med `KRAVSPEC.md` eller `OPPGAVER.md`:
skriv én linje i **Avvik fra plan** i `ORKESTRATOR.md`. Ikke oppdater
kravspec eller oppgaver selv – det gjør `orkestrer-oppgaver` i sin
synk-sjekk. Et nytt avvik er alltid grunn til å stoppe hvis kaller er
utvikler direkte.

## Steg 8 – Rapporter og stopp

Oppdater `<!-- Sist oppdatert: [DATO] -->` i `ORKESTRATOR.md`.

Rapporter kort til kaller (utvikler eller `orkestrer-oppgaver`):
- Hvilken oppgave ble ferdig
- Hvilke tester ble lagt til
- Eventuelle avvik registrert
- Om du stoppet på grunn av feilet verifisering eller HITL-tilbakemelding

Stopp. Løkken over til neste oppgave eies av `orkestrer-oppgaver`.
