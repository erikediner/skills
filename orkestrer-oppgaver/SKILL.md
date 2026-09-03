---
name: orkestrer-oppgaver
description: Kjører løkken over kanban i `OPPGAVER.md`, én oppgave om gangen, og synker plan mot virkelighet.
disable-model-invocation: true
---

# orkestrer-oppgaver

Du er løkken over kanban-brettet. Du velger neste oppgave, delegerer selve
utførelsen til `implementer`, synker plan mot virkelighet, og tømmer
AFK-batchen ved naturlige stopp. Du gjør ikke TDD selv – det er `implementer`
sin jobb.

## Hvem eier hva

- **Kanban i `OPPGAVER.md` eier status og fremdrift.** Klar / Pågår / Ferdig
  er eneste sannhet, og kolonnen **Pågår** er selve markøren for
  gjenopptaking. Avvik, AFK-batch og synk-historikk står i egne seksjoner
  nederst i samme fil. Ingen egen tilstandsfil.
- **`implementer` eier én oppgave** – rød-grønn-syklus, selvverifisering,
  HITL-demo eller AFK-batch-linje.

Ved gjenopptaking: les kanban i `OPPGAVER.md` – status og finposisjon står
begge der.

## Steg 1 – Finn grunnlaget

Be utvikler peke på oppgavemappen `docs/oppgaver/<oppgave-id>/`. Let etter:
- `KRAVSPEC.md` (fra `grill-kravspec`)
- `OPPGAVER.md` (fra `splitt-oppgaver`)

Hvis kravspec eller oppgaver mangler: stopp og be utvikler kjøre riktig skill først.

## Steg 2 – Velg neste oppgave

Fra kanban i `OPPGAVER.md`:
- Hopp over alt under **Ferdig**
- Hvis noe står under **Pågår**: ta den (gjenoppta)
- Ellers: ta første under **Klar** hvor alle blokkerende oppgaver er ferdige

Hvis brettet er tomt: tøm AFK-batchen til utvikler og stopp.

## Steg 3 – Deleger til `implementer`

Start en subagent som bruker `implementer`-skillen med den valgte oppgaven som
input. `implementer` gjør alt per-oppgave-arbeid: HITL-innsjekk, TDD-delegering,
selvverifisering, flytting til Ferdig, batch-oppdatering, avviksregistrering.

Vent på rapport tilbake. Hvis `implementer` stoppet på grunn av feilet
verifisering eller HITL-tilbakemelding: stopp løkken og gi kontrollen til
utvikler.

## Steg 4 – Synk-sjekk

Se på **Avvik fra plan**-seksjonen nederst i `OPPGAVER.md`. Hvis det finnes nye
avvik siden forrige synk: stopp og foreslå konkrete oppdateringer av
`KRAVSPEC.md` og/eller `OPPGAVER.md` (hva, hvor, hvorfor). Vent på godkjenning
per punkt, anvend endringene, og logg under **Synk-historikk** med dato. Et
nytt avvik stopper alltid løkken – også midt i en AFK-rekke.

Hvis utvikler trigget skillen i kun-synk-modus («synk planen»): kjør kun dette
steget og stopp.

## Steg 5 – Loop eller stopp

Gå tilbake til steg 2 og ta neste oppgave. Stopp løkken når:
- Alle oppgaver er ferdige (tøm batchen til utvikler)
- Neste oppgave er HITL (tøm batchen, be om go før du starter `implementer`)
- Utvikler ber om pause (tøm batchen)

Avslutt alltid med å sette inn dagens dato i `<!-- Sist oppdatert: [DATO] -->`
øverst i `OPPGAVER.md` – til nytte for mennesker som leser filen. Kanban
(kolonnene Klar/Pågår/Ferdig) er det som faktisk forteller neste kjøring
hvor den er.
