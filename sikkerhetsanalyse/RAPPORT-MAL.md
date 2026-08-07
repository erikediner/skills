# Sikkerhetsanalyse: <prosjektnavn>

<!-- Generert av KI med menneskelig supervensjon. Sist oppdatert: [DATO] -->

## Sammendrag

- Antall funn: **X** (kritisk: X, høy: X, medium: X, lav: X)
- Største risiko: ...
- Anbefalt første tiltak: ...

## Scope

- Kodebase: ...
- Analysert: STRIDE pr. komponent, OWASP Top 10, NSM SSL-utvalg, light dependency-sjekk
- IKKE dekket: full CVE-skann, penetrasjonstest, linje-for-linje review

## Angrepsoverflate

Kort beskrivelse av komponentene som ble vurdert (API, frontend, jobber,
integrasjoner, datakilder, hemmeligheter).

## Funn – prioritert (JIRA-klare)

Hvert funn er strukturert som en JIRA-oppgave og kan kopieres rett inn.
Prioritets-mapping: **Kritisk → Highest**, **Høy → High**, **Medium → Medium**,
**Lav → Low**. Estimat: **S = 1 SP**, **M = 3 SP**, **L = 8 SP**.

### Kritisk

#### K1 – <kort tittel>

```jira
Summary:     [SEC] <kort tittel>
Issue Type:  Bug
Priority:    Highest
Components:  <modul / område>
Labels:      security, stride-<kategori>, owasp-<kode>
Estimat:     S / M / L
```

**Status (lokalt)**: Åpen

**Description**

`<sti/fil.ext:linje>` — kort beskrivelse av svakheten i klartekst.

_Hvordan oppdaget_: STRIDE (f.eks. `E – Elevation of Privilege`) / OWASP
(f.eks. `A01 Broken Access Control`) / NSM SSL / GDPR.

_Risiko_: Hvem kan utnytte dette, og hva er konsekvensen?

**Acceptance Criteria**

- [ ] Konkret kodeendring som lukker hullet
- [ ] Test som verifiserer at angrepet ikke lenger virker
- [ ] Ingen tilsvarende svakhet i naboendepunkter/komponenter

**Anbefalt tiltak**

Konkret handling – ikke «forbedre logging», men «logg `userId` og `action`
på alle PUT/DELETE i `/api/admin/*`».

---

### Høy

#### H1 – ...

(samme JIRA-block-struktur, `Priority: High`)

### Medium

#### M1 – ...

(samme JIRA-block-struktur, `Priority: Medium`)

### Lav

#### L1 – ...

(samme JIRA-block-struktur, `Priority: Low`)

## Dependency-observasjoner (light)

| Pakke | Versjon | Siste | Kommentar |
|---|---|---|---|
| ... | ... | ... | ... |

Anbefalt full skann: `<kommando, f.eks. npm audit>`

## NSM SSL – observasjoner

| Område | Status | Kommentar |
|---|---|---|
| Hemmelighetshåndtering | OK / Avvik | ... |
| Logging & monitorering | OK / Avvik | ... |
| Patche-strategi | OK / Avvik | ... |
| Tilgangsstyring | OK / Avvik | ... |
| Sikker konfigurasjon | OK / Avvik | ... |

## Personvern (GDPR / Datatilsynet)

Behandles personopplysninger: **Ja / Nei**. Hvis ja:

| Krav | Status | Kommentar |
|---|---|---|
| Behandlingsgrunnlag dokumentert | OK / Avvik | ... |
| Dataminimering | OK / Avvik | ... |
| Lagringsbegrensning / sletting | OK / Avvik | ... |
| Innebygd personvern (kryptering, pseudonymisering) | OK / Avvik | ... |
| Registrertes rettigheter (innsyn, retting, sletting) | OK / Avvik | ... |
| Tredjepartsoverføring ut av EØS | OK / Avvik / N/A | ... |
| Avviksvarsling (72-timersrutine) | OK / Avvik | ... |

Tydelige brudd er løftet inn under **Funn – prioritert** ovenfor.

## Antagelser og åpne spørsmål

- ...

## Endringslogg

- [DATO] Første versjon
