# Spec F6: Preview

**Issue:** [#39](https://github.com/mikeleg/kets-cmsm/issues/39)

## Problem Statement

Il Client vuole vedere una Page sul frontend del Site prima che la vedano i visitatori. I casi sono due: una Page in Draft e una modifica non salvata a una Page Published. Una Page Published non ha una bozza delle modifiche, quindi il Client non ha altro modo di controllare il risultato prima di "Salva".

## Solution

L'admin manda da solo all'API la copia di lavoro della Page, dopo ogni modifica e con un breve ritardo. L'API la tiene in memoria sotto un token di 30 minuti. Il frontend del Site legge la Page con `?preview=<token>` e mostra una barra "Anteprima". Un token non cambia mai quello che vedono i visitatori.

## User Stories

1. Come Client, voglio vedere una Page in Draft sul frontend del Site prima di pubblicarla, per controllarla come la vedranno i visitatori.
2. Come Client, voglio vedere una modifica non salvata a una Page Published sul frontend del Site, per controllarla prima che vada online.
3. Come Client, voglio aprire la Preview con un pulsante nell'admin, per non costruire l'URL a mano.
4. Come Client, voglio che la Preview riceva da sola l'ultima modifica, per non vedere una versione vecchia.
5. Come Client, voglio un messaggio chiaro quando la Preview è scaduta, per sapere che devo riaprirla dall'admin.
6. Come Client, voglio un avviso nella Preview di una Page figlia di una Page in Draft, per capire perché i visitatori non la vedono.
7. Come visitatore, voglio che una Preview non cambi mai quello che vedo, per non vedere contenuti non pronti.
8. Come sviluppatore del frontend, voglio un contratto chiaro per la Preview, per implementarlo in ogni Site.

## Implementation Decisions

- Modulo Preview: un archivio in memoria `token → copia di lavoro della Page`, con una scadenza di 30 minuti rinnovata a ogni invio.
- Un token vale solo per la sua Page. Aprire un'altra Page nell'admin chiude il token precedente.
- L'admin invia la copia di lavoro dopo ogni modifica, con un breve ritardo. Non c'è un pulsante "Aggiorna anteprima".
- API pubblica: `?preview=<token>` restituisce la copia di lavoro, mai in cache. Un token scaduto o sconosciuto ha una risposta dedicata, che il frontend mostra come "Anteprima scaduta".
- Il meccanismo è lo stesso per una Page in Draft e per una modifica non salvata a una Page Published.
- Il resto del Site, in Preview, usa i dati salvati. La regola della voce di Menu in Preview è in F7 (slice 7.2).
- Obblighi del frontend: riconosce `?preview=`, non mette in cache quelle risposte, mostra una barra "Anteprima".
- L'URL del frontend del Site serve all'admin per aprire la Preview.

## Testing Decisions

- Seam principale: l'API HTTP con `WebApplicationFactory`. Copre token, scadenza, rinnovo, un token per Page e nessun effetto sui visitatori.
- Gli scenari vengono dal prototipo `docs/prototypes/anteprima`.
- Nessun test sul frontend del Site, che è fuori dal CMS.

## Out of Scope

- Bozza salvata delle modifiche a una Page Published.
- Preview condivisa con altre persone.
- Uno starter per il frontend del Site.

## Further Notes

- Viene dalla spec generale #17 e dalla decisione #8.
- Lo scenario "Anteprima vecchia" del prototipo ha portato all'invio automatico.
