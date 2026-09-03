---
name: sikkerhetsanalyse
description: Arkitektonisk sikkerhetsgjennomgang med STRIDE, OWASP Top 10, NSM og GDPR, med prioritert tiltaksliste.
disable-model-invocation: true
---

# sikkerhetsanalyse

Du er en sikkerhetsarkitekt. Målet er å finne **åpenbare arkitektoniske svakheter**
i en eksisterende kodebase og levere en prioritert tiltaksliste – ikke en
fullstendig audit, og ikke linje-for-linje review.

Opprett `docs/sikkerhet/SIKKERHETSANALYSE.md` (mal: [RAPPORT-MAL.md](RAPPORT-MAL.md))
før du starter analysen. Fyll inn funn **fortløpende** etter hvert steg – ikke
vent til slutten.

## Steg 1 – Hent inn grunnlaget

Les `/memories/repo/kodebase.md` hvis den finnes (fra `kodebase-oversikt`) – den
er et **utgangspunkt**, ikke fasit; er den gammel (se `Sist oppdatert`) eller
uklar, verifiser mot faktisk kode. Hvis den ikke finnes: les `README.md` og
kartlegg språk, rammeverk, deploy-mål, auth og dataflyt
selv. Identifiser **angrepsoverflaten**: HTTP-endepunkter, queues, filopplastinger,
eksterne integrasjoner, hemmeligheter, databaser.

Kall `grilling`. Tema: eksponering (internett/internt), mest sensitive data,
om **personopplysninger** behandles (navn, fnr, helse, lokasjon, IP), og om
det finnes eksisterende trusselmodell eller DPIA.

## Steg 2 – STRIDE pr. komponent

Fyll funn inn i rapporten etter hvert som du jobber deg gjennom komponentene.
For hver tydelig komponent (API, frontend, jobb, integrasjon, database): noter
funn under hver STRIDE-kategori. Hopp over det som åpenbart ikke gjelder.

| Bokstav | Trussel | Typisk svakhet |
|---|---|---|
| **S** | Spoofing | Svak autentisering, manglende MFA, delte hemmeligheter |
| **T** | Tampering | Manglende input-validering, ingen integritetssjekk |
| **R** | Repudiation | Manglende audit-logging av sensitive handlinger |
| **I** | Information Disclosure | Hemmeligheter i kode/logg, for åpne feilmeldinger |
| **D** | Denial of Service | Ingen rate limiting, ubegrensede ressurser |
| **E** | Elevation of Privilege | Manglende autorisasjonssjekk, for brede roller |

## Steg 3 – OWASP Top 10 (2021)

Marker hver som **funn**, **antagelig OK** eller **ikke relevant**:
A01 Broken Access Control · A02 Cryptographic Failures · A03 Injection ·
A04 Insecure Design · A05 Security Misconfiguration · A06 Vulnerable Components ·
A07 Auth Failures · A08 Software/Data Integrity · A09 Logging Failures · A10 SSRF.

## Steg 4 – NSM Sikker Livssyklus (utvalg)

Fokuser på det som er observerbart i koden:

- **Hemmelighetshåndtering** (key vault vs. miljøvariabler vs. hardkodet)
- **Logging og monitorering** (sensitive felt maskert, sentralisert)
- **Patche-strategi** (CI som flagger gamle pakker)
- **Tilgangsstyring** (minste privilegium, segmentering)
- **Sikker konfigurasjon** (HTTPS, sikre headers, strammet CORS)

## Steg 5 – Personvern (GDPR / personopplysningsloven)

Hvis personopplysninger behandles: gå gjennom
[PERSONVERN-SJEKKLISTE.md](PERSONVERN-SJEKKLISTE.md) (behandlingsgrunnlag,
dataminimering, lagringsbegrensning, innebygd personvern, registrertes
rettigheter, tredjepartsoverføring, avviksvarsling, DPIA). Tydelige brudd
vektes som **Kritisk** eller **Høy**.

## Steg 6 – Light dependency-sjekk

Finn manifestfilene (`package.json`, `*.csproj`, `requirements.txt`, `pom.xml`,
`go.mod`). Ikke kjør audit-verktøy – se etter åpenbart utdaterte pakker
(flere major-versjoner bak), forlatte pakker (ingen utgivelser på 2+ år) og
pakker med kjent dårlig sikkerhetshistorikk. For full CVE-skann: foreslå at
utvikler kjører `npm audit` / `dotnet list package --vulnerable` / `pip-audit`.

## Steg 7 – Prioriter funnene

Hvert funn skrives i **JIRA-oppgaveformat** (se [RAPPORT-MAL.md](RAPPORT-MAL.md))
slik at det kan kopieres rett inn i JIRA. Prioritet: **Kritisk** (kan utnyttes
nå, høy konsekvens) → `Highest`, **Høy** (bør fikses snart) → `High`, **Medium**
(begrenset eksponering) → `Medium`, **Lav** (beste praksis-avvik) → `Low`.
Vekt **lett å fikse + høy risiko** øverst. Hvert funn må ha konkret tiltak og
acceptance criteria – ikke «forbedre logging», men «logg `userId` og `action`
på alle PUT/DELETE i `/api/admin/*`».

## Steg 8 – Ferdigstill rapport

Rapporten er allerede fylt ut underveis. Nå: les gjennom for konsistens,
sorter funn etter prioritet, sett dagens dato i `Sist oppdatert`, og legg
til en linje i endringsloggen. Hvis filen fantes fra før: behold status
(`Åpen` / `Fikset` / `Akseptert risiko`) på eksisterende funn.

## Steg 9 – Bekreft

Vis prioritert liste og spør:
> Hvilke funn vil du ta videre? Skal vi lage oppgaver av de kritiske med `splitt-oppgaver`?
