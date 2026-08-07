# Sjekkliste for catalog-info

Bruk denne sjekklisten foer du skriver eller oppdaterer `catalog-info.yaml`.

## 1) Entitetsnivaa og struktur

- Bruk `apiVersion: backstage.io/v1alpha1`.
- Del entiteter med `---`.
- Ha minst ett `System` for helheten.
- Modellér kjorbar programvare som `Component`.
- Modellér API-er som `API`.
- Modellér databaser og filsystem som `Resource`.

## 2) Kind/type-regler

- Backend: `kind: Component`, `spec.type: service`.
- Frontend: `kind: Component`, `spec.type: website`.
- Eget bibliotek: `kind: Component`, `spec.type: library`.
- REST/OpenAPI: `kind: API`, `spec.type: openapi`.
- Databaseskjema: `kind: Resource`, `spec.type: database`.
- Filsystem/lager: `kind: Resource`, `spec.type: filsystem`.

## 3) Metadata og tekst

- `metadata.name` skal vaere unik, lesbar og kebab-case.
- Ingen mellomrom eller norske spesialtegn i id.
- `metadata.description` skal vaere paa norsk.
- Beskrivelser skal vaere korte og meningsbaerende.

## 4) Referanser og avhengigheter

- Referer med unike id-er, ikke URL-er eller servernavn.
- `dependsOn` for databaser/filsystem/bibliotek.
- `consumesApis` for API-er komponenten bruker.
- `providesApis` for API-er komponenten leverer.
- Ikke dupliser samme avhengighet i samme liste.

## 5) API-definition

- `spec.definition` er paakrevd for `kind: API`.
- Foretrekk OpenAPI-lenke som returnerer JSON.
- Hvis definisjon mangler: bruk
  `definition: 'server : https://example.com/api'`.

## 6) Logisk nivå, ikke infrastruktur

Ikke modellér:
- servere, hostnames, porter eller runtime-instanser
- fysisk plassering (lokalt/sky) som avhengighet
- tabeller/kolonner i databaseskjema
- filserverdetaljer

Modellér:
- logiske tjenestenavn
- logiske databasenavn (skjema)
- logiske filsystemnavn

## 7) Scope for biblioteker

- Ta med biblioteker dere selv vedlikeholder.
- Ikke ta med tredjepartsbiblioteker.

## 8) Oppdateringsmodus (eksisterende fil)

- Behold etablerte id-er hvis mulig.
- Gjør minimal diff.
- Oppdater eksisterende blokk fremfor a legge ny, hvis samme id.
- Fjern aapenbart feil data kun med begrunnelse.

## 9) Ny fil (hvis mangler)

- Opprett `catalog-info.yaml` i repo-rot.
- Start med System + minst en Component.
- Legg til API/Resource kun ved dekning i kode eller dokumentasjon.

## 10) Sluttkontroll

- Ingen dubletter i `metadata.name`.
- Alle referanser peker til eksisterende id-er.
- Eier (`owner`) og `lifecycle` er satt konsekvent.
- Filen kan kjores gjennom skillen pa nytt uten duplikater.
