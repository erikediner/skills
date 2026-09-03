---
name: feilsoking
description: >
  Feilsøker en konkret feil: bygger en rød tilbakemeldingssløyfe som
  reproduserer akkurat denne feilen før noen hypotese formuleres, minimerer,
  instrumenterer, fikser, og skriver regresjonstest. Brukes når utvikler sier
  "feilsøk", "debug", eller når noe kaster en feil, feiler eller er
  uventet tregt.
---

# feilsoking

Du feilsøker én konkret feil. Målet er en sløyfe som pålitelig går rød på
akkurat dette symptomet – før du gjetter på årsak. Ingen fiks uten en
reproduksjon som beviser at fiksen faktisk endrer noe.

## Sikkerhet – gjelder gjennom hele feilsøkingen

Skjul hemmeligheter (tokens, passord, API-nøkler, personopplysninger) i alt
du viser fram: logger, feilmeldinger, HTTP-forespørsler, skjermbilder. Bygg
sløyfer som leser hemmeligheter fra miljøvariabler (`$env:`, `.env`, secret
manager) – lim aldri en faktisk nøkkel inn i en kommando, fil eller melding
til utvikler.

## Fase 1 – Bygg den røde sløyfen (hoveddelen)

Før du formulerer én eneste hypotese: bygg en tilbakemeldingssløyfe som
reproduserer feilen pålitelig og raskt. Denne sløyfen er verktøyet du bruker
resten av feilsøkingen – den må kunne kjøres på nytt på sekunder, ikke minutter.

Ranger metodene i denne rekkefølgen og bruk den første som er mulig:

1. **Feilende test på nærmeste snitt-punkt** – finnes det allerede en test
   som feiler, eller kan du skrive én raskt mot det offentlige grensesnittet
   nærmest feilen? Raskest å iterere på, og blir regresjonstesten i fase 6.
2. **HTTP-kall mot kjørende dev-server** – `curl` / `Invoke-RestMethod` mot
   endepunktet med input som trigger feilen. Bruk når feilen sitter i et
   API-lag og en test krever for mye oppsett.
3. **CLI-kjøring mot fast input** – kjør kommandoen eller scriptet direkte
   med et fast, minimalt input-sett som trigger feilen.
4. **Headless nettleserskript** – for feil som bare oppstår i UI/DOM. Naviger
   til siden, utfør handlingen, fang feilen.
5. **Avspilling av en lagret forespørsel** – siste utvei: en HAR-fil, et
   request-dump eller en logget hendelse fra produksjon/staging, spilt av
   lokalt.

Bekreft at sløyfen faktisk går **rød** på symptomet før du går videre. Går
den ikke rød: du har ikke reprodusert feilen ennå. Fortsett på dette steget
– ikke gjett deg videre uten reproduksjon.

## Fase 2 – Minimer

Fjern alt fra reproduksjonen som ikke er nødvendig for at den går rød:
irrelevante felt i input, uvedkommende kode-stier, unødvendig oppsett. Et
minimalt eksempel gjør neste steg presist i stedet for vagt.

## Fase 3 – Hypotese

Formuler én konkret hypotese om årsaken, basert på det minimerte eksempelet
og koden du har lest. Én hypotese om gangen – ikke en liste med gjetninger.

## Fase 4 – Instrumenter

Bekreft eller avkreft hypotesen med et logg-punkt, en debugger, eller en
assert plassert der hypotesen sier problemet er. Ikke fiks før du har
bekreftet – en ubekreftet hypotese er fortsatt en gjetning.

## Fase 5 – Fiks

Gjør minimal endring som løser den bekreftede årsaken. Kjør den røde sløyfen
fra fase 1 på nytt – den skal nå gå grønn.

## Fase 6 – Skriv regresjonstest

Gjør reproduksjonen fra fase 1 til en permanent test i testsuiten, hvis den
ikke allerede var en test. Testen skal feile mot koden slik den var før
fiksen, og passere etter.

## Rapporter tilbake

Kort til utvikler: hva var feilen (fil + linje), hva var årsaken, hva var
fiksen, hvilken regresjonstest ble lagt til.
