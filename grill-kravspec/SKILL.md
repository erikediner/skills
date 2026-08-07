---
name: grill-kravspec
description: >
  Griller utvikler om én spesifikk ny oppgave eller feature og skriver en ferdig
  kravspesifikasjon med nøkkelbegreper og arkitekturavgjørelser innbakt. Brukes
  når PO har gitt en konkret oppgave, eller utvikler sier "grill meg",
  "lag kravspesifikasjon", "hjelp meg forstå oppgaven". IKKE bruk for å kartlegge
  en hel kodebase – bruk `kodebase-oversikt` da. IKKE bruk for å splitte arbeidet
  i tasks – det gjør `splitt-oppgaver`.
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

Grill utvikler grundig om alle aspekter av oppgaven. Still spørsmål **ett om gangen**
med anbefalt svar, og vent på svar før neste. Jobb deg gjennom designtreet – ta én gren
om gangen og løs avhengigheter mellom beslutninger underveis. Fortsett til du har nok
til å skrive en fullstendig kravspesifikasjon.

Teknikker:
- **Utforsk koden før du spør** – hvis svaret finnes i repoet, ikke spør
- **Skjerp uklare ord** – når utvikler bruker vage eller overlastede termer, foreslå et presist begrep
- **Test med konkrete scenarioer** – tving frem grensetilfeller før de blir antagelser

Oppdater [KRAVSPEC-MAL.md](KRAVSPEC-MAL.md) fortløpende når et begrep eller en beslutning
blir avklart – ikke vent til slutten. Da blir steg 3 bare polering.

## Steg 3 – Ferdigstill kravspesifikasjon

Dokumentet er allerede fylt ut underveis. Nå skal du:

- Lese gjennom for konsistens i begrepsbruk
- Fylle ut eventuelle huller
- Lagre som `docs/oppgaver/<oppgave-id>/KRAVSPEC.md`. Opprett mappen hvis den
  ikke finnes. Hele oppgavens dokumentasjon (kravspec, oppgaver, orkestrator)
  vil bo i denne mappen.

## Steg 4 – Bekreft

Presenter kravspesifikasjonen og spør:
> Er dette en felles forståelse av oppgaven? Skal noe justeres før vi går videre?
