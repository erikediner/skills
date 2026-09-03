---
name: grilling
description: >
  Intervjuer utvikler i runder for å avklare et tema før videre arbeid.
  Stiller hele frontlinjen av spørsmål som kan besvares nå, nummerert med
  anbefalt svar, og venter på svar før neste runde. Kalles av
  `grill-kravspec`, `splitt-oppgaver` og `sikkerhetsanalyse` når de trenger
  å avklare noe med utvikler. Brukes når utvikler sier "grill meg om X"
  eller når en annen skill trenger et intervju.
---

# grilling

Du intervjuer utvikler om et tema en annen skill har gitt deg. Å finne fakta
er din jobb – utforsk kode, dokumentasjon og kontekst selv før du spør.
Beslutninger er brukerens jobb – ikke gjett deg til dem.

## Input fra kaller

Kaller oppgir **temaet** for intervjuet (hva skal avklares) og eventuelt hvor
svarene skal skrives (f.eks. en seksjon i en mal). Utforsk selv det som kan
avklares uten å spørre, før du lager spørsmål.

## Runder, ikke ett spørsmål om gangen

Bygg en **frontlinje**: alle spørsmål som kan besvares **nå**, uavhengig av
hverandre. Still dem samlet, nummerert, hver med et anbefalt svar. Vent på
svar på hele runden før du går videre.

Et spørsmål som avhenger av svaret på et annet, ubesvart spørsmål hører
**ikke** hjemme i denne runden – det kommer i en senere runde, når det
avhengige spørsmålet er avklart.

## Løkke

1. Utforsk kode, dokumentasjon og kontekst for det som allerede kan besvares uten å spørre
2. Bygg frontlinjen: spørsmål uten uavklarte avhengigheter, nummerert, med anbefalt svar
3. Still hele runden samlet, vent på svar
4. Oppdater grunnlaget med svarene, skjerp vage eller overlastede ord til presise begrep
5. Bygg neste frontlinje ut fra det som nå er avklart
6. Gjenta til frontlinjen er tom

## Ferdig

Frontlinjen er tom – ingen flere spørsmål kan stilles uten at noe nytt dukker
opp. Rapporter kort til kaller hva som ble avklart, og skriv resultatet dit
kaller ba om.
