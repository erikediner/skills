---
name: kodegjennomgang
description: >
  Gjennomgår en avgrenset diff mot repoets standarder og mot kravene den
  skal oppfylle. Tar et fast punkt i git-historikken, kjører to subagenter i
  parallell (standarder, krav) i egen kontekst, og presenterer funnene
  atskilt. Brukes når utvikler sier "kodegjennomgang", "gjennomgå diffen",
  "sjekk koden mot kravspec", eller når `implementer` kaller den etter grønne
  tester. IKKE for arkitektonisk sikkerhetsgjennomgang – se
  `sikkerhetsanalyse`. IKKE for å skrive commit-meldinger eller oppsummere
  endringer – bare for å vurdere dem mot standarder og krav.
---

# kodegjennomgang

Du gjennomgår en diff i to uavhengige subagenter – én for standarder, én for
krav – slik at gjennomgangen skjer i egen kontekst og ikke gjenbruker
konteksten som skrev koden.

## Steg 1 – Fastsett diffen

Kaller oppgir et fast punkt i git-historikken («før»-punktet: commit-SHA, tag
eller branch). Kjør `git rev-parse <punkt>` for å bekrefte at det løses.
Løser det ikke: stopp, be kaller oppgi et gyldig punkt.

Kjør `git diff <punkt>...HEAD`. Er diffen tom: stopp, meld at det ikke er
noe å gjennomgå.

Finn `KRAVSPEC.md` og gjeldende oppgave i `OPPGAVER.md` under
`docs/oppgaver/<oppgave-id>/`, hvis de finnes.

## Steg 2 – Kjør to subagenter i parallell

Start begge samtidig. Gi hver subagent kun diffen (og for B: kravspec og
oppgaven) som input – ikke resten av samtalen som skrev koden.

**Subagent A – Standards**

Spør: følger diffen repoets dokumenterte standarder (README, instructions-
filer, linter-/formatter-config)? Hopp over alt lint og typesjekk allerede
fanger. Sjekk i tillegg alltid mot denne faste listen, uansett hva repoet
selv dokumenterer:
- Duplisert logikk med små variasjoner i stedet for én abstraksjon
- Funksjon eller klasse som gjør mer enn én ting
- Dyp nesting (mer enn 2–3 nivåer)
- Magisk tall eller streng uten navngitt konstant
- Feil som svelges stille (tom catch, logget og glemt)
- Navn som ikke beskriver hensikt
- Kommentar som forklarer hva koden gjør, ikke hvorfor
- Død kode eller kode kommentert ut
- Abstraksjon innført uten nåværende behov

**Subagent B – Krav**

Spør: gjør diffen det `KRAVSPEC.md` og oppgaven i `OPPGAVER.md` ba om? Gi
den diffen, kravspec og oppgavebeskrivelsen. Rapporter tre kategorier:
- Krav som mangler (bedt om, ikke bygget)
- Ting som er bygget uten å være bedt om
- Krav som ser implementert ut, men er feil (feil betingelse, feil
  datakilde, feil grensetilfelle)

## Steg 3 – Presenter funnene

Vis de to rapportene under hver sin overskrift:

```
## Standards
<funn fra subagent A>

## Krav
<funn fra subagent B>
```

Ikke slå sammen rapportene og ikke ranger funn på tvers av dem. Hver
rapport står for seg.

## Steg 4 – Rapporter tilbake

Gi kaller (utvikler eller `implementer`) begge rapportene samlet. Har noen
av dem funn: la kaller vurdere om det stopper flyten videre – f.eks. før
oppgaven flyttes til Ferdig i `implementer`.
