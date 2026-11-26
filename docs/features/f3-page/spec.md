# Spec F3: Page

**Issue:** [#36](https://github.com/mikeleg/kets-cmsm/issues/36)

## Problem Statement

Il Client vuole cambiare i testi del suo Site, aggiungere Page e organizzarle senza aiuto dello sviluppatore. Non deve vedere online una Page a metà, non deve rompere i link già indicizzati e non deve cancellare per errore una parte del Site.

## Solution

Il Client lavora in un editor visivo: la Page disegnata al centro e il form del Block in un pannello laterale. Le modifiche vanno in una copia di lavoro e un solo "Salva" le porta online. Una Page nuova parte in Draft e il Client la pubblica quando è pronta. Le Page formano un albero e l'URL nasce dal percorso degli slug. Un cambio di URL crea un redirect 301. Una Page eliminata risponde 410. L'API tiene una cache in memoria che si svuota a ogni salvataggio.

## User Stories

1. Come Client, voglio aprire la Home nell'editor e vederla disegnata come sul Site, per capire dove cade ogni modifica.
2. Come Client, voglio modificare i Field di un Block in un pannello laterale, per vedere la Page mentre scrivo.
3. Come Client, voglio vedere "Modifiche non salvate", per sapere che devo ancora salvare.
4. Come Client, voglio un solo "Salva" per tutta la Page, per salvare insieme tutte le modifiche.
5. Come Client, voglio che ogni salvataggio di una Page Published vada online subito, per non avere passaggi in più.
6. Come Client, voglio vedere "Online subito" sul pulsante di una Page Published, per sapere cosa succede quando salvo.
7. Come visitatore, voglio vedere una modifica salvata dalla mia prossima richiesta, per avere sempre il contenuto aggiornato.
8. Come sviluppatore del frontend, voglio un `ETag` e le risposte `304`, per ridurre il traffico se lo voglio.
9. Come Client, voglio creare una Page nuova, per aggiungere una sezione al mio Site.
10. Come Client, voglio che una Page nuova parta in Draft, per costruirla senza che i visitatori la vedano a metà.
11. Come Client, voglio un badge "Draft: i visitatori non la vedono", per sapere lo stato della Page.
12. Come Client, voglio pubblicare una Page in Draft con un solo gesto, per metterla online quando è pronta.
13. Come Client, voglio riportare in Draft una Page Published, per toglierla dal Site senza eliminarla.
14. Come Client, voglio scegliere la Page da modificare da un menu a tendina, per passare da una Page all'altra in fretta.
15. Come visitatore, voglio che una Page in Draft risponda 404, per non trovare contenuti non pronti.
16. Come Client, voglio mettere una Page sotto un'altra Page, per organizzare il Site ad albero (es. Servizi > Consulenza).
17. Come Client, voglio che lo slug nasca dal titolo, per non doverlo scrivere a mano.
18. Come Client, voglio cambiare lo slug di una Page in "Proprietà della Page", per avere URL leggibili.
19. Come visitatore, voglio che le figlie di una Page in Draft non siano visibili, per non trovare Page orfane.
20. Come Client, voglio spostare una Page nell'albero, per riorganizzare il Site.
21. Come Client, voglio un avviso prima di spostare una Page, per sapere che il suo URL e quello delle sue figlie cambiano.
22. Come visitatore, voglio che un vecchio URL mi porti al nuovo con un redirect 301, per non trovare link rotti.
23. Come Client, voglio che la Home sia sempre all'URL `/` e che non si possa eliminare né spostare, per non rompere il Site.
24. Come Client, voglio eliminare una Page senza figlie, per togliere un contenuto che non serve più.
25. Come Client, voglio una conferma dentro la pagina prima di eliminare una Page, per non eliminarla per errore.
26. Come Client, voglio che l'eliminazione di una Page con figlie sia bloccata, per non cancellare un ramo intero.
27. Come visitatore, voglio una risposta 410 sull'URL di una Page eliminata, perché i motori di ricerca la tolgano.

## Implementation Decisions

- Modulo Contenuto: Page, albero, slug, redirect e stato Draft/Published.
- Editor visivo, variante B del prototipo `docs/prototypes/editor-page`: la Page disegnata al centro, il form del Block in un pannello laterale, la scelta della Page da un menu a tendina, "Proprietà della Page" (titolo, slug, Page padre) nel pannello.
- Un solo "Salva" per Page, sulla copia di lavoro. "Pubblica" salva e porta la Page in Published. Spostare una Page vale subito, fuori da "Salva", dopo un avviso.
- Nessuna finestra di dialogo del browser.
- Draft/Published è uno stato per Page, comune a tutte le Language (ADR 0004).
- Lo slug è un valore per Language (ADR 0004, decisione #9). La Language predefinita non ha prefisso nell'URL.
- API pubblica, lettura per percorso: la Page, un redirect 301 al nuovo URL, 404 (Page in Draft, figlia di una Page in Draft o inesistente) oppure 410 (Page eliminata).
- Ogni cambio di slug o di posizione di una Page Published crea un redirect 301 per la Page e per le sue figlie.
- Cache: le risposte pubbliche stanno in memoria nell'API e si svuotano a ogni salvataggio. Le letture pubbliche hanno un `ETag` e rispondono `304`. Raccomandazione per il frontend: nessuna cache delle pagine HTML, o pochi secondi.
- In questa feature l'editor lavora con il Block Testo creato da F1. Gli altri Block type e le operazioni sui Block sono in F4.

## Testing Decisions

- Seam principale: l'API HTTP, con `WebApplicationFactory` e un file SQLite temporaneo. Copre stati, albero, 301, 404, 410, cache ed `ETag`.
- Seam di supporto: pochi test Playwright contro l'immagine Docker: modificare e salvare la Home, creare e pubblicare una Page.
- Nessun test unitario dei componenti Svelte.

## Out of Scope

- Workflow di approvazione e bozza delle modifiche a una Page Published.
- Blog, news e tipi di Page diversi dalla lista di Block.
- Ripristino di una Page eliminata.
- Pubblicazione separata per Language.

## Further Notes

- Viene dalla spec generale #17 e dalle decisioni #2, #10 e #12.
- La conferma dell'eliminazione e l'avviso dello spostamento seguono il prototipo dell'editor.
