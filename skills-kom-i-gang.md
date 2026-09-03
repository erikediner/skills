# Skills: kom i gang

En praktisk gjennomgang for seksjonen. Målet er at du skal ha skrevet din første skill
før du er ferdig med å lese, og at du skal vite når det lønner seg.

Alt du trenger er GitHub Copilot og en teksteditor.

---

## 1. Hva en skill er

En skill er en mappe med en `SKILL.md`-fil. I fila står det hva skillen heter, når den
skal brukes, og hvordan man gjør det.

Det er alt. Ingen konfigurasjon, ingen installasjon, ingen plugin.

```markdown
---
name: grill-meg
description: Griller meg om en oppgave før noe blir skrevet. Brukes når jeg sier
  "grill meg".
---

Intervju brukeren nådeløst til dere har felles forståelse.
```

Det er hele fila. Fire linjer, og den endrer måten Copilot oppfører seg på.

### Skill, mal, instruksjonsfil

| | Hva den er | Når den leses |
|---|---|---|
| **Skill** | En framgangsmåte. «Slik gjør vi denne typen oppgave.» | Når oppgaven passer beskrivelsen |
| **Mal** | Et skjelett for resultatet. «Slik ser sluttproduktet ut.» | Når skillen viser til den |
| **Instruksjonsfil** (`copilot-instructions.md`) | Faste regler for repoet. | Hver eneste gang |

Instruksjonsfila leses alltid, og bør derfor holdes kort. Skills leses bare når de
trengs, og kan derfor være mange.

---

## 2. Hvorfor det virker

Copilot leser ikke alle skillsene dine ved oppstart. Den leser bare `name` og
`description`. Det koster nesten ingen kontekst.

Først når du ber om noe som passer en beskrivelse, leser den resten av fila. Og først
når instruksjonene viser til en hjelpefil, leser den den.

1. **Alltid lastet:** navn og beskrivelse. Noen få tokens per skill.
2. **Lastet ved treff:** hele `SKILL.md`.
3. **Lastet ved referanse:** maler, skript og eksempler i mappa.

Derfor kan du ha tjue skills liggende uten at svarene blir dårligere. Konteksten fylles
ikke før noe faktisk brukes. Det er også grunnen til at store maler skal ligge som egne
filer, ikke limes inn i `SKILL.md`.

---

## 3. Hvor filene ligger

**For ett prosjekt** (deles med alle som kloner repoet):

```
.github/skills/<skill-navn>/SKILL.md
```

`.claude/skills/` og `.agents/skills/` fungerer også, hvis skillen skal plukkes opp av
andre verktøy enn Copilot.

**For deg selv** (følger med på tvers av prosjekter):

```
~/.copilot/skills/<skill-navn>/SKILL.md
```

På Windows: `C:\Users\<brukernavn>\.copilot\skills\`.

### En felle som koster ti minutter

Mappenavnet og `name`-feltet må være identiske. Er de ikke det, lastes ikke skillen, og
du får ingen feilmelding. Du merker bare at ingenting skjer.

```
.github/skills/grill-meg/SKILL.md   →   name: grill-meg
```

---

## 4. Din første skill

Vi lager en som griller deg på en oppgave før noe blir skrevet. Det er den mest nyttige å
begynne med, og den er kort.

### Steg 1: Lag mappa

```bash
mkdir -p ~/.copilot/skills/grill-meg
```

### Steg 2: Skriv fila

Opprett `~/.copilot/skills/grill-meg/SKILL.md`:

```markdown
---
name: grill-meg
description: Griller meg om en oppgave før noe blir skrevet. Brukes når jeg sier
"grill meg", eller ber om hjelp til å planlegge en endring, en feature eller et dokument.

---

# Grill meg

Målet er felles forståelse før noe produseres.

## Regler

- Ikke skriv kode, planer eller utkast før spørsmålene er besvart
- Still spørsmålene i runder: hele frontlinjen av det som kan besvares nå,
  nummerert med et anbefalt svar. Vent på svar på hele runden før neste
- Fortsett til du ikke har flere vesentlige spørsmål. Det kan bli 5 eller 50
- Utfordre antakelser. Sier jeg "åpenbart", spør hvorfor

## Spør om

- Hva som faktisk skal løses, og for hvem
- Hva som er utenfor omfanget
- Hva dette berører av det som allerede finnes
- Hva "ferdig" betyr, konkret
- Hva som kan gå galt

## Til slutt

Oppsummer det dere er enige om i en kort liste, og be om bekreftelse.
```

### Steg 3: Prøv den

Åpne Copilot Chat eller Copilot CLI i Agent-modus og skriv:

```
/grill-meg Jeg skal lage en side som viser status på målestasjonene våre
```

Du kan også bare beskrive oppgaven uten skråstrek. Passer det du skriver med
`description`, plukker Copilot opp skillen selv.

### Steg 4: Legg merke til hva som skjer

Den begynner å spørre i stedet for å produsere. Erfaringen er at mange av
spørsmålene avdekker noe du ikke hadde tenkt ferdig på.

Det er hele poenget. Du får ikke nødvendigvis bedre svar av en flinkere modell. Du får bedre svar av å
ha tenkt ferdig først.

> **Vil du ikke skrive selv?** Det ligger en mer utbygd variant som `grill-kravspec` i
> [erikediner/skills](https://github.com/erikediner/skills). Kopier mappa til
> `~/.copilot/skills/` og kjør `/grill-kravspec`. Den utforsker kodebasen først og
> skriver en ferdig kravspec til slutt.

---

## 5. Beskrivelsen er kanskje det viktigste feltet

`description` er det eneste modellen ser når den avgjør om skillen er relevant. Alt annet
i fila er bortkastet hvis beskrivelsen ikke trigger.

En dårlig beskrivelse:

```yaml
description: Hjelper med kravspesifikasjoner.
```

Tre ting gjør en beskrivelse god:

**Si når, ikke bare hva.** «Brukes når PO har gitt en konkret oppgave.»

**Ta med de faktiske ordene folk skriver.** «grill meg», «lag kravspesifikasjon». Tenk på
beskrivelsen som en søkestreng, ikke en oppsummering.

**Avgrens mot naboene.** Så snart du har mer enn to skills, begynner de å trigge på
hverandres oppgaver. Løsningen er å si rett ut hva skillen *ikke* er til:

```yaml
description: Griller utvikler om én spesifikk ny oppgave eller feature og skriver en
  ferdig kravspesifikasjon. Brukes når PO har gitt en konkret oppgave, eller utvikler
  sier "grill meg", "lag kravspesifikasjon". IKKE bruk for å kartlegge en hel kodebase
  – bruk `kodebase-oversikt` da. IKKE bruk for å splitte arbeidet i tasks – det gjør
  `splitt-oppgaver`.
```

`IKKE bruk`-linjene er den enkeltendringen som gir mest når samlingen vokser. De peker
brukeren til riktig skill i stedet for at feil skill svarer.

**En fjerde ting, når samlingen vokser videre: skal modellen i det hele tatt få velge
skillen selv?** Sett `disable-model-invocation: true` i frontmatter for skills som er
tunge, endrer mye, eller har en beskrivelse som stadig kolliderer med naboenes. Da
starter du dem bare med `/skillnavn`, og beskrivelsen kan kortes ned til én menneskelig
linje i skill-velgeren i stedet for en søkestreng modellen skal treffe på. Hjelpe-skills
som bare kalles av andre skills – aldri av deg direkte – bør derimot forbli modellstyrt
med en rik beskrivelse, ellers har ingenting noe å starte dem med.

---

## 6. Maler: la skillen vise til et skjelett

Skillen beskriver framgangsmåten. Malen beskriver resultatet. Legg malen i samme mappe og
vis til den fra instruksjonene.

```
grill-meg/
├── SKILL.md
└── SPEC-MAL.md
```

I `SKILL.md`:

```markdown
Er oppgaven stor nok, skriv det dere ble enige om til SPEC.md etter SPEC-MAL.md
i denne mappa. Små oppgaver trenger ingen fil.
```

Og malen kan være så enkel som dette:

```markdown
# <Tittel>

## Mål
Én setning: hva skal være løst?

## Omfang
Innenfor:
Utenfor:

## Åpne valg
1.

## Ferdig når
- [ ]
```

Malen gir samme struktur hver gang, viser hva som mangler (er «Åpne valg» tom, har dere
ikke tenkt nok), og gjør at alle leser resultatet på samme måte.

Forbeholdet om at små oppgaver ikke trenger fil er viktig. Uten det lager skillen et
dokument for hver bagatell, og da slutter folk å bruke den.

**Et triks verdt å kopiere:** la skillen fylle ut malen *underveis*, ikke til slutt. Da
havner avklaringene i dokumentet mens de er ferske, og siste steg blir korrektur i stedet
for gjenskaping.

---

## 7. Flere skills som peker på hverandre

Én skill er nyttig. Flere som henger sammen er en arbeidsflyt. Et vanlig oppsett:

```
grill  →  splitt  →  implementer  →  test
```

**Grill** til dere er enige, og skriv det ned hvis oppgaven er stor nok.

**Splitt** i små, uavhengige biter. Hver bit bør være et tynt snitt gjennom hele
stacken, ikke «først all database, så all UI». Resultatet er en liste med Klar, Pågår og
Ferdig.

**Implementer** én bit om gangen. Ikke to. Én.

**Test** mot det som ble avtalt, ikke mot magefølelsen.

Poenget er ikke akkurat disse fire. Poenget er at hver skill gjør én ting, og at
resultatet fra den ene er inndata til den neste.

> **Et ferdig oppsett å se på:** [erikediner/skills](https://github.com/erikediner/skills)
> har hele kjeden implementert som `grill-kravspec`, `splitt-oppgaver`,
> `orkestrer-oppgaver`, `implementer` og `tdd`, pluss noen for å komme inn i en ukjent
> kodebase.

---

## 8. Velg riktig tyngde

Den vanligste feilen er å kjøre hele løpet på alt. Bygg avveiningen inn i skillen:

- **Feature-størrelse, eller noe er uavklart:** full spec, så oppdeling, så
  implementering
- **Lite og trivielt:** kort grilling, tre til fem linjer rett i chatten, og rett i gang

En ettlinjes bugfix skal ikke ha et spec-dokument. Er skillen i tvil, bør den spørre deg
hvilken tyngde du vil ha.

---

## 9. Slik lager du en skill uten å skrive den

Den enkleste måten å lage en god skill på er å ikke skrive den fra bunnen.

1. Løs en oppgave med Copilot helt som vanlig
2. Når du er fornøyd: **«Skriv ned hvordan vi kom hit, som en skill»**
3. Rydd litt i den, og legg den i skills-mappa
4. Bruk den neste gang. Må du rette noe, retter du **skillen**, ikke bare resultatet

Punkt fire er det som gjør at det vokser. Skillen blir bedre for hver gang, i stedet for
at du gjør den samme rettingen om igjen.

---

## 10. Prinsipper som har vist seg å holde

**Én skill, ett ansvar.** Sikkerhet bor i én skill, ikke spredt utover.

**Hold hver skill kort**, rundt 100 til 120 linjer. Store maler ligger som egne filer.

**Disjoint triggere.** `IKKE bruk når …` peker til riktig naboskill.

**Én eier av status.** Én fil eier hva som er gjort. Andre filer speiler den ikke.

**Velg ett språk og hold deg til det**, i beskrivelse, instruksjoner og resultat.

**Selvverifisering alltid.** Les diffen og kjør testene uansett om et menneske er med.

**Idempotent.** Skillen skal kunne kjøres på nytt uten å duplisere eller miste arbeid.

**Merk det som er generert:**
`<!-- Generert av KI med menneskelig supervensjon. Sist oppdatert: [DATO] -->`

---

## 11. Vanlige feil

**Skillen trigger aldri.** Sjekk at mappenavn og `name` er identiske. Sjekk deretter om
`description` inneholder ordene du faktisk skriver.

**Feil skill svarer.** Beskrivelsene overlapper. Legg til `IKKE bruk når …` med
henvisning til riktig naboskill.

**Skillen er 400 linjer.** Del den. Flytt maler og referansemateriale til egne filer i
mappa og vis til dem.

**Du skriver skills for ting du gjør én gang.** En skill lønner seg først når oppgaven
gjentar seg.

**Du stoler på resultatet fordi det ser bra ut.** Skills gjør ikke svaret riktig. De gjør
det lettere for deg å oppdage at det er feil. Ansvaret er fortsatt ditt.

---

## 12. Videre

**Se denne:** Matt Pococks
[Full Walkthrough: Workflow for AI Coding](https://www.youtube.com/watch?v=-QFHIoCo-Ko).
Halvannen time, og mye av arbeidsflyten over kommer derfra. Vil du hoppe rett i det:
grill-sessionen starter 12:45, oppdeling i vertikale snitt 35:50, og implementering med
agenter 48:15.

**Skills å kopiere fra:**

- [erikediner/skills](https://github.com/erikediner/skills) — våre, på norsk, med et
  komplett oppsett fra kravspec til tester
- [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering)
- [awesome-copilot](https://awesome-copilot.github.com/skills/)
- [anthropics/skills](https://github.com/anthropics/skills)

**Dokumentasjon:**

- [About agent skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- [Use Agent Skills in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-skills)
- [agentskills.io](https://agentskills.io) — spesifikasjonen

---

## Verdt å vite

Formatet er åpent. En `SKILL.md` skrevet for Copilot fungerer også i Claude, Cursor og
andre agenter. Bytter vi verktøy, følger skillsene med.

Skills fungerer i dag per repo eller per bruker. Skills på organisasjonsnivå er varslet
fra GitHub, men er ikke tilgjengelig ennå. Sjekk hva som er skrudd på hos oss før du
planlegger noe stort.
