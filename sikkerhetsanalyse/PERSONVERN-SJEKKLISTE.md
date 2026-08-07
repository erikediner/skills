# Personvern – sjekkliste (GDPR / personopplysningsloven)

Brukes av [SKILL.md](SKILL.md) steg 5. Bygger på kravene Datatilsynet håndhever.
Sjekk hvert punkt og noter status (OK / Avvik / N/A) med kort begrunnelse.
Tydelige brudd løftes inn som funn i hovedrapporten med prioritet **Kritisk**
eller **Høy**.

## Behandlingsgrunnlag

- Er det dokumentert hvilket grunnlag (samtykke, avtale, lovhjemmel, berettiget
  interesse) hver behandling hviler på?
- For samtykke: er det frivillig, spesifikt, informert og lett å trekke tilbake?

## Dataminimering

- Samles det inn flere felt enn det formålet krever?
- Logges personopplysninger (e-post, fnr, IP, posisjon) som ikke trenger å logges?
- Brukes ekte persondata i test/dev-miljø?

## Lagringsbegrensning

- Finnes det definerte sletteregler eller anonymisering, eller bare evig lagring?
- Slettes data også fra backup, søkeindekser, logger og cacher?

## Innebygd personvern (privacy by design)

- Krypteres sensitive felt i ro (database, filer, backup)?
- Pseudonymiseres data der mulig (hash, tokenisering)?
- Er default-innstillinger satt til mest personvernvennlige valg?

## Registrertes rettigheter

- Finnes mekanismer for innsyn, retting, sletting og dataportabilitet?
- Er det mulig å levere ut alle data om én person uten manuell graving?

## Tredjepartsoverføring

- Sendes data til land utenfor EU/EØS?
- Hvis ja: finnes gyldig overføringsgrunnlag (adekvansbeslutning, SCC, BCR)?
- Er underleverandører (databehandlere) listet og dekket av databehandleravtale?

## Avviksvarsling

- Finnes rutine for å oppdage og varsle Datatilsynet innen 72 timer ved brudd?
- Er det definert hvem som er behandlingsansvarlig og hvem som er kontaktpunkt?

## DPIA (vurdering av personvernkonsekvenser)

- Krever behandlingen DPIA (høy risiko, sensitive kategorier, store volum)?
- Hvis ja: finnes den, og er den oppdatert mot dagens implementasjon?
