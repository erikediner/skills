---
name: forbedre-catalog-info
description: >
  Finner eksisterende catalog-info.yaml/yml i et repo, validerer innholdet mot
  NVE-regler for Backstage, leser dokumentasjon og relevant kode, og oppdaterer
  filen med manglende eller viktig informasjon med minimal diff. Brukes når
  utvikler vil "sjekke catalog-info", "forbedre backstage-fil", "oppdatere
  catalog-info", "validere programvarekatalog", eller ber om kvalitetssjekk av
  Backstage-spesifikasjon.
---

# forbedre-catalog-info

Mål: Gjøre `catalog-info.yaml` mer korrekt, komplett og nyttig uten å bryte
establerte id-er eller duplisere entiteter.

Last [SJEKKLISTE.md](SJEKKLISTE.md) før du starter.

## Steg 1 - Finn fil og modus

- Sjekk om `catalog-info.yaml` eller `catalog-info.yml` finnes.
- Hvis ingen finnes: opprett `catalog-info.yaml` i repo-rot.
- Hvis fil finnes: jobb i oppdateringsmodus med minimal diff.

## Steg 2 - Kartlegg kilder

Les dette i prioritert rekkefølge:
- `README.md` og docs under `docs/`
- Arkitektur- og driftsdokumentasjon i repoet
- Manifest og konfig (`*.csproj`, `package.json`, `appsettings*.json`, env-filer)
- Viktig kode for avhengigheter (API-klienter, database, filsystem, interne bibliotek)

Hvis en sentral opplysning mangler, still korte oppklaringsspørsmål ett om
gangen med anbefalt svar.

## Steg 3 - Valider mot regler

Valider mot [SJEKKLISTE.md](SJEKKLISTE.md):
- riktige `kind`/`spec.type`
- unike kebab-case id-er i `metadata.name`
- logiske avhengigheter (ikke infrastrukturdetaljer)
- norske beskrivelser
- API `definition` satt

Flagg avvik tydelig, og foreslå konkret retting.

## Steg 4 - Oppdater filen

- Behold eksisterende `metadata.name` der det er mulig.
- Fjern ikke gyldig informasjon uten grunn.
- Når du er i ferd med å opprette en **ny** entitet (System, Component, API,
  Resource): spør om det allerede finnes en entitet med samme navn eller ansvar
  registrert fra et annet repo i Backstage. Hvis ja – referer til den i stedet
  for å opprette en duplikat.
- Legg til manglende system/component/api/resource.
- Oppdater referanser i `dependsOn`, `consumesApis`, `providesApis`.
- Hvis Swagger/OpenAPI mangler, bruk:
  `definition: 'server : https://example.com/api'`

Vis diff i chat for skriving til disk ved stor endring.

## Steg 5 - Kjørbar på nytt

Sikre idempotens:
- ikke dupliser entiteter med samme id
- ikke bytt id-er uten eksplisitt grunn
- oppdater eksisterende blokker fremfor å lage nye

## Kort eksempel

```yaml
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
  name: min-backend
  description: Backend for saksbehandling.
spec:
  type: service
  owner: nve
  lifecycle: production
  dependsOn:
    - resource:saksdata-db
```
