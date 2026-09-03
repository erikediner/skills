---
name: grill-kravspec
description: Griller utvikler om én ny oppgave og skriver en ferdig kravspesifikasjon.
disable-model-invocation: true
---

# grill-kravspec

Du er en senior utvikler og kravanalytiker. Målet er å nå felles forståelse av
oppgaven før én eneste linje kode skrives — og deretter skrive en komplett kravspesifikasjon.

## Omfang – velg riktig tyngde

De fleste oppgaver fortjener en full kravspec. Men for små, lavrisiko-endringer
(f.eks. en ettlinjes bugfix eller en triviell justering) er full spec +
`splitt-oppgaver` + `orkestrer-oppgaver` overkill.

- **Feature-størrelse, eller noe er uavklart:** kjør hele løpet under (full spec
  → `splitt-oppgaver` → `orkestrer-oppgaver`).
- **Lite og trivielt:** gjør en kort grilling for å bekrefte forståelsen, skriv
  et mini-spec rett i chatten (3–5 linjer: hva, hvorfor, hvordan verifisere) og
  send det direkte til `tdd`. Hopp over `splitt-oppgaver` og `orkestrer-oppgaver`.

Er du i tvil, spør utvikler hvilken tyngde de vil ha.

## Steg 1 – Forstå utgangspunktet

Be utvikler om:
- Oppgavebeskrivelsen (tekst fra PO, JIRA-kort, e-post, muntlig)
- Hvilken del av kodebasen det gjelder (om kjent)
- **Oppgave-id:** JIRA-nummer hvis det finnes (f.eks. `PROJ-123`), pluss et kort
  beskrivende navn (f.eks. `elektrisk-fakturering`). Bygg `<oppgave-id>` som
  `<JIRA-NR>-<kort-navn>` hvis JIRA finnes, ellers bare `<kort-navn>`.

Utforsk deretter kodebasen selv. Sjekk først `/memories/repo/kodebase.md` hvis
den finnes – den er et **utgangspunkt**, ikke fasit. Hvis dokumentet er gammelt
(se `Sist oppdatert`) eller noe er uklart, verifiser mot faktisk kode før du
stoler på det:
- Les `README.md` og eventuelle modul-README-er
- Finn relevante filer, typer, grensesnitt og tester i berørt område
- Identifiser eksisterende begreper og mønstre

## Steg 2 – Grill (viktigste steg)

Kall `grilling`. Tema: alle aspekter av oppgaven som trengs for en full
kravspesifikasjon – avgrensning, brukerhistorier, datamodeller,
feilhåndtering, grensetilfeller, ikke-funksjonelle krav. Be den skjerpe vage
eller overlastede termer til presise begrep, og teste med konkrete
scenarioer for å tvinge frem grensetilfeller.

Oppdater [KRAVSPEC-MAL.md](KRAVSPEC-MAL.md) fortløpende når en runde gir svar
– ikke vent til slutten. Da blir steg 4 bare polering.

## Steg 3 – Avtal snitt-punkter

Skisser hvilke **snitt-punkter** (offentlige grensesnitt du kan observere atferd
gjennom, uten å nå inn i implementasjonen) oppgaven skal testes mot – ett per
vertikalt snitt om flere er aktuelle.

- Foretrekk et eksisterende snitt-punkt fremfor å innføre et nytt
- Velg det høyest mulige snitt-punktet (nærmest slik bruker/klient faktisk
  observerer systemet), ikke et internt hjelpelag
- Skriv dem opp som en kort liste og få dem **eksplisitt godkjent av utvikler**
  her – `tdd` henter dem senere fra kravspecen og spør ikke om dette på nytt

Fyll listen inn i seksjonen **Snitt-punkter** i [KRAVSPEC-MAL.md](KRAVSPEC-MAL.md).

## Steg 4 – Ferdigstill kravspesifikasjon

Dokumentet er allerede fylt ut underveis. Nå skal du:

- Lese gjennom for konsistens i begrepsbruk
- Fylle ut eventuelle huller
- Lagre som `docs/oppgaver/<oppgave-id>/KRAVSPEC.md`. Opprett mappen hvis den
  ikke finnes. Hele oppgavens dokumentasjon (kravspec, oppgaver) vil bo i
  denne mappen.

## Steg 5 – Bekreft

Presenter kravspesifikasjonen og spør:
> Er dette en felles forståelse av oppgaven? Skal noe justeres før vi går videre?
