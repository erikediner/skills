---
name: kode-gjennomgang
description: >
  To-akse review av diff mot et fast punkt (commit, branch, merge-base):
  Standard (følger koden repoets dokumenterte standarder?) og Spec (dekker
  koden det KRAVSPEC.md/oppgaven ba om?). Parallelle subagenter, ett samlet
  funn. Brukes når utvikler ber om review av en branch eller PR, eller når
  `implementer` avslutter en oppgave.
---

# kode-gjennomgang

To-akse review av diff mellom `HEAD` og et fast punkt.

- **Standard** — følger koden repoets dokumenterte kodestandarder og mønstre?
- **Spec** — dekker koden det `KRAVSPEC.md` (eller oppgaven) ba om?

Begge akser kjøres som **parallelle subagenter** slik at de ikke forurenser
hverandres kontekst. En endring kan passere én akse og feile den andre —
aksene er bevisst separate og skal ikke veies mot hverandre.

## Steg 1 – Fest det faste punktet

Brukeren eller `implementer` oppgir et fast punkt — commit-SHA, branch-navn,
`main`, `HEAD~5` osv. Mangler det: spør.

Verifiser at punktet finnes (`git rev-parse <punkt>`) og at diff-en ikke er
tom (`git diff <punkt>...HEAD`). Feiler ett av disse: stopp her.

## Steg 2 – Finn spesifikasjonen

Let etter spec-kilde i denne rekkefølgen:
1. `KRAVSPEC.md` i oppgavemappen (`docs/oppgaver/<oppgave-id>/`)
2. Issue-referanse i commit-meldingene
3. PRD/spec-fil under `docs/` eller `specs/`

Mangler alt: spør brukeren. Sier de at det ikke finnes spec, hopper
Spec-subagenten over og rapporterer «ingen spec tilgjengelig».

## Steg 3 – Finn standardkildene

Let etter filer som dokumenterer hvordan kode skal skrives:
`CODING_STANDARDS.md`, `CONTRIBUTING.md`, `docs/konvensjoner*` eller tilsvarende.

## Steg 4 – Start begge subagenter parallelt

Send én melding med to subagent-kall (general-purpose).

**Standard-subagent** — gi med:
- Diff-kommandoen og commit-listen
- Funnet standardkilde(r)
- Oppgave: «Rapportér per fil/hunk: (a) steder diff-en bryter en dokumentert
  standard — siter filen og regelen; (b) mønstre som avviker fra repoets
  tydelige konvensjoner (navngivning, arkitektur, feilhåndtering). Skill klare
  brudd fra skjønnsbaserte observasjoner. Maks 400 ord.»

**Spec-subagent** — gi med:
- Diff-kommandoen og commit-listen
- Spec-innholdet eller filsti
- Oppgave: «Rapportér: (a) krav spec-en ba om som mangler eller er delvise;
  (b) atferd i diff-en som spec-en ikke ba om (scope creep); (c) krav som ser
  implementert ut men ser gale ut. Siter spec-linjen per funn. Maks 400 ord.»

## Steg 5 – Samle og presenter

Presenter under `## Standard` og `## Spec` — ordrett eller lett redigert.
**Ikke slå sammen eller rangér på tvers** av aksene.

Avslutt med én linje: antall funn per akse og det viktigste funnet innen
hver akse (om noen).
