---
name: tdd
description: >
  Implementerer én oppgave (ett vertikalt snitt) med rød-grønn-refaktor og
  tracerkule-tilnærming. Tester atferd gjennom offentlig grensesnitt, ikke
  implementasjonsdetaljer. Brukes når utvikler sier "kjør tdd", "test-først",
  "implementer med tdd", eller når `orkestrer-oppgaver` delegerer en oppgave.
---

# tdd

Du implementerer én oppgave – ett vertikalt snitt – ved hjelp av rød-grønn-refaktor.
Du tester atferd gjennom offentlige grensesnitt, ikke implementasjonsdetaljer.

## Snitt-punkter – hvor tester hører hjemme

Et **snitt-punkt** er det offentlige grensesnittet du tester atferd mot: der du
kan observere hva systemet gjør uten å nå inn i det.

**Ingen test skrives mot et ubekreftet snitt-punkt.** Før én test lages: skriv ned
hvilke snitt-punkter som er aktuelle og bekreft dem med utvikler eller orkestrator.
Du kan ikke teste alt — å avtale snitt-punktene på forhånd er det som sikrer at
testinnsatsen lander på kritiske stier og kompleks logikk, ikke tilfeldige kanter.

Spør: «Hva er det offentlige grensesnittet, og hvilke snitt-punkter skal vi teste?»

## Filosofi

**God test:** beskriver _hva_ systemet gjør gjennom et snitt-punkt. Overlever
refaktorering fordi den ikke bryr seg om intern struktur. Leses som en spesifikasjon.
Forventede verdier kommer fra en **uavhengig kilde** — et kjent-godt literal,
et utregnet eksempel, spec-en — aldri reberegnet på samme måte som koden.

**Dårlig test:** koblet til implementasjon. Mocker interne samarbeidspartnere, tester
private metoder, eller verifiserer ved å gå utenom snitt-punktet. Bryter når du refaktorerer
selv om atferden er uendret.

## Antimønster: tautologisk test

En tautologisk test beregner forventet verdi på **samme måte** som koden gjør det:
`expect(sum(a, b)).toBe(a + b)`. Den passerer alltid, gir null forsikring, og kan
aldri avdekke en feil. Forventede verdier **må** komme fra en uavhengig kilde —
et kjent-godt literal, et håndregnet eksempel, en verdi fra spec-en.

## Antimønster: horisontal snitting

**Ikke skriv alle testene først, og så all implementasjonen.** Det produserer dårlige tester
som tester forestilt atferd og er ufølsomme for ekte endringer.

```
FEIL (horisontalt):
  RØD:   test1, test2, test3, test4
  GRØNN: impl1, impl2, impl3, impl4

RIKTIG (vertikalt – tracerkule):
  RØD → GRØNN: test1 → impl1
  RØD → GRØNN: test2 → impl2
  ...
```

## Arbeidsflyt

### 1. Plan

Før noe kode skrives:

- Bekreft hvilke grensesnittendringer som trengs
- **Avtal snitt-punktene** — skriv dem ned og få godkjenning
- Prioriter atferd som skal testes (ikke implementasjonssteg)
- Bruk prosjektets begrepsbruk fra kravspec og README

Spør: «Hva er snitt-punktene, og hvilke atferder er viktigst å teste?»

### 2. Tracerkule

Skriv ÉN test som bekrefter ÉN ting:

```
RØD:   Skriv test for første atferd → testen feiler
GRØNN: Minimal kode for å bestå → testen passerer
```

Dette beviser at veien gjennom alle lag fungerer ende-til-ende.

### 3. Inkrementell løkke

For hver gjenstående atferd:

```
RØD:   Skriv neste test → feiler
GRØNN: Minimal kode for å bestå → passerer
```

Regler:
- Én test om gangen
- Bare nok kode til å bestå nåværende test
- Ikke foregrip fremtidige tester
- Hold tester på observerbar atferd

### 4. Refaktor

Når alle testene er grønne:

- Fjern duplikasjon
- Skjul kompleksitet bak enkle grensesnitt
- Kjør testene etter hvert refaktoreringssteg

**Aldri refaktorer mens RØD.** Få det grønt først.

## Sjekkliste per syklus

- [ ] Testen beskriver atferd, ikke implementasjon
- [ ] Testen bruker bare offentlig grensesnitt
- [ ] Testen ville overlevd en intern refaktorering
- [ ] Koden er minimal for denne testen
- [ ] Ingen spekulativ funksjonalitet lagt til

## Rapportering tilbake

Når oppgaven er ferdig, rapporter kort til den som kalte deg (utvikler eller orkestrator):

- Hvilke tester ble lagt til (filnavn + testnavn)
- Hvilken atferd dekkes nå
- Eventuelle avvik fra planen
- Forslag til oppfølging hvis du oppdaget teknisk gjeld
