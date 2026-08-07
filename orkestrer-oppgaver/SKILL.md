---
name: orkestrer-oppgaver
description: >
  Løkke over kanban i `OPPGAVER.md`: velger neste oppgave, delegerer
  utførelsen til `implementer`, synker `KRAVSPEC.md`/`OPPGAVER.md` mot
  virkeligheten underveis, og tømmer AFK-batch ved naturlige stopp.
  Resumerbar (Ralph-stil). Brukes når utvikler sier "start orkestrator",
  "fortsett implementeringen", "kjør resten av oppgavene", "synk planen".
  For utførelse av én enkelt oppgave, se `implementer`. For nedbryting av
  kravspec i oppgaver, se `splitt-oppgaver`.
---

# orkestrer-oppgaver

Du er løkken over kanban-brettet. Du velger neste oppgave, delegerer selve
utførelsen til `implementer`, synker plan mot virkelighet, og tømmer
AFK-batchen ved naturlige stopp. Du gjør ikke TDD selv – det er `implementer`
sin jobb.

## Hvem eier hva

- **Kanban i `OPPGAVER.md` eier status.** Klar / Pågår / Ferdig er eneste
  sannhet.
- **`ORKESTRATOR.md` eier fremdrift og hukommelse:** markør, logg, avvik,
  synk-historikk, AFK-batch. Ingen speiling av kanban.
- **`implementer` eier én oppgave** – rød-grønn-syklus, selvverifisering,
  HITL-demo eller AFK-batch-linje.

Ved gjenopptaking: les status fra kanban, finposisjon fra markøren.

## Steg 1 – Finn grunnlaget

Be utvikler peke på oppgavemappen `docs/oppgaver/<oppgave-id>/`. Let etter:
- `KRAVSPEC.md` (fra `grill-kravspec`)
- `OPPGAVER.md` (fra `splitt-oppgaver`)
- `ORKESTRATOR.md` (denne skillens egen tilstandsfil)

Hvis kravspec eller oppgaver mangler: stopp og be utvikler kjøre riktig skill først.

## Steg 2 – Last eller opprett orkestratorfil

Hvis `ORKESTRATOR.md` finnes i oppgavemappen: les den og bruk markøren.
Ellers: lag den fra [ORKESTRATOR-MAL.md](ORKESTRATOR-MAL.md).

## Steg 3 – Velg neste oppgave

Fra kanban i `OPPGAVER.md`:
- Hopp over alt under **Ferdig**
- Hvis noe står under **Pågår**: ta den (gjenoppta)
- Ellers: ta første under **Klar** hvor alle blokkerende oppgaver er ferdige

Hvis brettet er tomt: tøm AFK-batchen til utvikler og stopp.

## Steg 4 – Deleger til `implementer`

Start en subagent som bruker `implementer`-skillen med den valgte oppgaven som
input. `implementer` gjør alt per-oppgave-arbeid: HITL-innsjekk, TDD-delegering,
selvverifisering, flytting til Ferdig, batch-oppdatering, avviksregistrering.

Vent på rapport tilbake. Hvis `implementer` stoppet på grunn av feilet
verifisering eller HITL-tilbakemelding: stopp løkken og gi kontrollen til
utvikler.

## Steg 5 – Synk-sjekk

Se på **Avvik fra plan** i orkestratorfilen. Hvis det finnes nye avvik siden
forrige synk: stopp og foreslå konkrete oppdateringer av `KRAVSPEC.md` og/eller
`OPPGAVER.md` (hva, hvor, hvorfor). Vent på godkjenning per punkt, anvend
endringene, og logg under **Synk-historikk** med dato. Et nytt avvik stopper
alltid løkken – også midt i en AFK-rekke.

Hvis utvikler trigget skillen i kun-synk-modus («synk planen»): kjør kun dette
steget og stopp.

## Steg 6 – Loop eller stopp

Gå tilbake til steg 3 og ta neste oppgave. Stopp løkken når:
- Alle oppgaver er ferdige (tøm batchen til utvikler)
- Neste oppgave er HITL (tøm batchen, be om go før du starter `implementer`)
- Utvikler ber om pause (tøm batchen)

Avslutt alltid med å oppdatere markøren i orkestratorfilen og sett inn dagens
dato i `<!-- Sist oppdatert: [DATO] -->` slik at neste kjøring vet hvor den er.
