# Spec F1: Installation e aggiornamenti

**Issue:** [#34](https://github.com/mikeleg/kets-cmsm/issues/34)

## Problem Statement

Lo sviluppatore di mgsoluzioni gestisce un'Installation per ogni Site. Oggi non ha un modo unico per avviare il CMS, servire i contenuti al frontend e portare una versione nuova su tutte le Installation. Un aggiornamento sbagliato può rompere un Site o perdere i suoi dati.

## Solution

Ogni Installation è un solo container Docker con un volume. Il container contiene un'API .NET Minimal API su SQLite, che serve anche l'admin SvelteKit. L'API pubblica restituisce la Home al frontend del Site, con un contratto OpenAPI. All'avvio l'API copia il database e applica le migrazioni. Lo sviluppatore pubblica l'immagine con una versione semver e aggiorna le Installation una alla volta con uno script, che torna indietro da solo se qualcosa fallisce.

## User Stories

1. Come sviluppatore, voglio avviare un'Installation con un solo container e un volume, per installare il CMS di un Site nuovo in pochi minuti.
2. Come sviluppatore del frontend, voglio leggere la Home dall'API pubblica con il percorso `/`, per mostrare la prima Page del Site.
3. Come sviluppatore del frontend, voglio un contratto OpenAPI dell'API pubblica, per generare il client nella tecnologia che preferisco.
4. Come sviluppatore del frontend, voglio passare `lang` all'API pubblica già dall'MVP, per non cambiare il frontend quando arriva una Language nuova.
5. Come sviluppatore, voglio che ogni testo sia salvato con un valore per Language, per aggiungere una Language dopo senza migrare i dati.
6. Come sviluppatore, voglio che un'Installation nuova abbia già una Home Published con un Block Testo, per vedere subito il Site funzionare.
7. Come sviluppatore, voglio avviare il progetto .NET e avere l'admin SvelteKit con hot reload, per sviluppare in fretta.
8. Come sviluppatore, voglio che l'admin stia sullo stesso dominio dell'API, per non gestire CORS e cookie tra domini diversi.
9. Come sviluppatore, voglio che database e media stiano in un volume, per non perdere i dati quando sostituisco il container.
10. Come sviluppatore, voglio una copia del file SQLite prima di ogni migrazione, per tornare indietro se la migrazione fallisce.
11. Come sviluppatore, voglio che il CMS tenga solo le ultime 3 copie, per non riempire il volume.
12. Come sviluppatore, voglio che l'API non parta se una migrazione fallisce, per non servire un Site con un database a metà.
13. Come sviluppatore, voglio un'immagine Docker con una versione semver su GitHub Container Registry, per installare e aggiornare ogni Installation allo stesso modo.
14. Come sviluppatore, voglio che ogni Installation fissi la sua versione e non usi mai `latest`, per sapere sempre cosa gira su ogni Site.
15. Come sviluppatore, voglio uno script che aggiorna le Installation una alla volta e si ferma al primo errore, per non rompere tutti i Site insieme.
16. Come sviluppatore, voglio che lo script rimetta la copia del database e la versione precedente se l'aggiornamento fallisce, per tornare indietro senza perdere dati.
17. Come sviluppatore, voglio che lo script segnali quale Installation ha fallito e perché, per correggere il problema.
18. Come sviluppatore, voglio che basti un host con Docker e un volume persistente, per scegliere l'hosting più tardi.

## Implementation Decisions

- Una Installation per ogni Site (ADR 0001): un container, un volume con il database SQLite e la Media library.
- CMS headless (ADR 0002). L'API .NET Minimal API espone l'API pubblica e l'API dell'admin. Il contratto OpenAPI copre l'API pubblica.
- L'admin SvelteKit si compila con `adapter-static` e lo serve l'API .NET sullo stesso dominio. In sviluppo lo SPA proxy di ASP.NET avvia il dev server di SvelteKit.
- Modello dei dati minimo di questa feature: Page e una tabella Block comune (Page, posizione, Block type) con il contenuto dei Field in una colonna JSON (ADR 0003). Ogni valore di testo è un oggetto per Language, es. `{"it": "Chi siamo"}` (ADR 0004).
- Al primo avvio il CMS crea la Home, Published, con un Block Testo.
- L'API pubblica legge una Page per percorso. Il parametro `lang` vale `it` se manca.
- All'avvio: copia del file SQLite (il CMS tiene le ultime 3), poi le migrazioni EF Core. Se una migrazione fallisce, l'API non parte.
- Immagine Docker su GitHub Container Registry con versione semver. Ogni Installation fissa la sua versione.
- Uno script aggiorna un elenco di Installation una alla volta. Si ferma al primo errore, ripristina la copia del database, rimette la versione precedente e segnala l'errore.
- Il deploy del frontend del Site è fuori dal CMS.

## Testing Decisions

- Un buon test controlla solo il comportamento visibile dall'esterno, attraverso le richieste HTTP o il browser. Niente mock delle classi interne.
- Seam principale: l'API HTTP del CMS, con test in-process `WebApplicationFactory` su un file SQLite temporaneo per ogni test.
- Seam di supporto: Playwright contro l'immagine Docker, solo per aprire l'admin.
- Lo script di aggiornamento si prova con una migrazione che fallisce apposta.
- Non ci sono test esistenti da prendere come modello: il repo non ha ancora codice.

## Out of Scope

- La scelta dell'hosting.
- Il backup delle Installation (#16). Resta solo la copia prima delle migrazioni.
- Il deploy del frontend del Site e uno starter per il frontend.
- Una vista centrale di tutti i Site.
- Aggiornamenti automatici delle Installation.

## Further Notes

- Viene dalla spec generale #17 e dalle decisioni #7 e #9 della mappa #1.
- Questa feature è la base di tutte le altre: la slice 1.1 non ha blocchi.
