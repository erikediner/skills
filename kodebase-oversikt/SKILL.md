---
name: kodebase-oversikt
description: >
  Onboarder utvikler til en ukjent kodebase som helhet: språk, arkitektur,
  begreper, patterns og uvanlig oppsett. Genererer eller oppdaterer
  README. Brukes når utvikler arver et prosjekt, skal sette seg inn i en hel
  kodebase, eller sier "gi meg oversikt", "hva gjør dette prosjektet", "lag readme".
  IKKE bruk for å forstå en spesifikk ny oppgave – bruk `grill-kravspec` da.
  IKKE bruk for sikkerhets-/sårbarhetsgjennomgang – bruk `sikkerhetsanalyse` da.
---

# kodebase-oversikt

Du er en erfaren utvikler som kartlegger en ukjent kodebase og skriver klar dokumentasjon.

## Steg 1 – Kartlegg kodebasen

Start med manifestfiler (`package.json`, `*.csproj`, `pom.xml`, `requirements.txt` osv.),
rotnivå-README og mappestrukturen for å få et overblikk. Følg opp tråder som ser
interessante eller uvanlige ut – og gå dypere når noe ikke gir mening enda.

Finn:

- **Språk og rammeverk** – primærspråk, versjoner, byggsystem
- **Arkitektur** – monolitt / mikroservice / lag-delt / modulær etc.
- **Nøkkelbegreper** – domeneord, mønster-navn, forkortelser som brukes internt
- **Patterns** – repository-pattern, event-driven, CQRS, DI osv.
- **Uvanlig oppsett** – avvik fra konvensjoner, særegne konfigurasjoner, utdaterte avhengigheter

> Merker du noe sikkerhetsrelatert i forbifarten (hardkodet hemmelighet,
> åpenbart utdatert pakke): noter det som et stikkord, men ikke gå i dybden her.
> Den fulle gjennomgangen (STRIDE, OWASP, personvern, dependencies) er jobben
> til `sikkerhetsanalyse`, som kjøres etter denne.

Sjekk alltid:
- Eksisterende `README.md`-filer (rot og moduler)
- Konfigurasjonsfiler (`.env*`, `docker-compose*`, CI-filer)
- Avhengighetsfiler (`package.json`, `requirements.txt`, `*.csproj` osv.)

## Steg 2 – Avklar uklarheter med brukeren

Etter kartleggingen: still kortfattede spørsmål om ting kodebasen ikke avslørte.

Typiske eksempler:
- Domeneforståelse: «Hva er forskjellen på [BegrepA] og [BegrepB] i denne konteksten?»
- Arkitekturhensikt: «Hvorfor er [modul X] skilt ut fra [modul Y]?»
- Ubrukt kode: «Ser ut som [fil/modul] ikke er i bruk – er den aktiv eller kan den ignoreres?»
- Miljøer: «Finnes det staging- eller prod-konfig et annet sted enn i repoet?»

Regler:
- Maks 4 spørsmål om gangen, gruppert som en liste
- Gi et forslag til svar der det er naturlig
- Vent på svar før du går videre til steg 3 (utkast)

## Steg 3 – Vis utkast i chat

Skriv et første utkast av README direkte i chatten basert på malen i
[README-MAL.md](README-MAL.md). Ikke lagre noen fil enda.

- Fyll ut alle seksjoner du har dekning for
- Merk seksjoner med `<!-- TODO -->` der informasjon mangler – ikke gjett
- Sett inn dagens dato i `<!-- Generert av KI med menneskelig supervensjon. Sist oppdatert: [DATO] -->`

## Steg 4 – Spør om plassering og oppsplitting

Basert på det du fant, anbefal én av disse og be om bekreftelse:

- a) Én README.md på rotnivå
- b) Én README.md per modul, lenket fra rot-README
- c) Begge deler

> Eksempel: «Kodebasen er en monolitt med tre tydelige moduler. Jeg anbefaler (b) – ok?»

Hvis en README.md allerede finnes: spør om du skal **oppdatere den** eller **lage en ny fil** (f.eks. `OVERSIKT.md`).

## Steg 5 – Lagre

Lagre utkastet der brukeren valgte. Hvis splittet per modul: lag rot-README med
lenker til hver modul-README.

## Steg 6 – Oppsummer funn

Etter at README er skrevet, gi en kort oppsummering (5–10 kulepunkter) av:
- Det viktigste å vite om kodebasen
- Uvanlig oppsett som kan overraske nye utviklere
- Teknisk gjeld som bør adresseres snart

Hvis du fanget opp sikkerhetsstikkord i steg 1: nevn dem i én linje og anbefal
`sikkerhetsanalyse` som neste steg. Ikke prioriter eller utdyp dem her.

## Steg 7 – Skriv nøkkelfakta til repo-memory

Lagre kjernefakta i `/memories/repo/kodebase.md` slik at påfølgende skills
(særlig `grill-kravspec`) slipper å gjenta utforskningen. Hold det kort – maks
~20 linjer. Inkluder:

- Primærspråk + versjon, hovedrammeverk
- Arkitekturmønster (én setning)
- Bygg- og testkommandoer
- 3–5 sentrale domenebegreper
- 1–2 ting som overrasker nye utviklere

Hvis filen finnes fra før: oppdater den i stedet for å duplisere. Sett inn
`Sist oppdatert: [DATO]` øverst.

