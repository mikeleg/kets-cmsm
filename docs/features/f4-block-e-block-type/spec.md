# Spec F4: Block e Block type

**Issue:** [#37](https://github.com/mikeleg/kets-cmsm/issues/37)

## Problem Statement

Il Client compone le Page con sezioni pronte, ma non deve poter rompere il design del Site. Lo sviluppatore deve dare a ogni Site i Block type che gli servono senza toccare il codice del CMS, che resta una sola immagine per tutte le Installation. Un errore nel file dei Block type non deve mai far perdere contenuti né togliere una Page dal Site.

## Solution

Il CMS ha un registro dei Block type: il set base sta nel CMS, i Block type di un solo Site stanno in un file JSON dell'Installation. Il registro produce i descrittori dei Field e l'admin costruisce ogni form da questi descrittori, con un componente per tipo di Field. Il Client aggiunge, riordina, duplica ed elimina Block nell'editor. I cambi al file seguono regole che non eliminano mai contenuti.

## User Stories

1. Come Client, voglio aggiungere un Block a una Page scegliendo da un elenco di Block type, per comporre la Page.
2. Come Client, voglio inserire un Block in un punto preciso, con il "+" tra due Block, per non doverlo spostare dopo.
3. Come Client, voglio riordinare i Block di una Page, per cambiare l'ordine delle sezioni.
4. Come Client, voglio duplicare un Block, per creare una sezione simile più in fretta.
5. Come Client, voglio eliminare un Block e annullare l'eliminazione per 7 secondi, per rimediare a un clic sbagliato senza finestre di conferma.
6. Come Client, voglio che un Field obbligatorio vuoto blocchi il salvataggio e apra il Block con l'errore, per sapere cosa manca.
7. Come Client, voglio formattare il testo solo con grassetto, corsivo, link, elenchi, H2 e H3, per non rompere il design.
8. Come Client, voglio un Link che punta a una Page del Site o a un URL esterno, con la sua etichetta, per collegare i contenuti.
9. Come Client, voglio una Scelta da un elenco fisso (es. immagine a sinistra o a destra), per variare il layout senza toccare il CSS.
10. Come Client, voglio compilare una Lista (es. le domande delle FAQ), per gestire elementi ripetuti.
11. Come Client, voglio usare i Block type Testo, Call to action e FAQ, per costruire le Page più semplici.
12. Come sviluppatore del frontend, voglio che il contratto OpenAPI descriva ogni Block type con i suoi Field, per generare un client tipizzato.
13. Come sviluppatore, voglio descrivere un Block type per un solo Site in un file JSON accanto al suo frontend, per tenere insieme il Block type e il componente che lo mostra.
14. Come sviluppatore, voglio uno JSON Schema del file dei Block type, per avere suggerimenti ed errori nell'editor.
15. Come sviluppatore, voglio scegliere quali Block type del set base sono attivi per un Site, per mostrare al Client solo quelli utili.
16. Come sviluppatore, voglio cambiare l'etichetta di un Field senza toccare i dati, per correggere un testo dell'admin.
17. Come sviluppatore, voglio che un file dei Block type non valido blocchi l'avvio con un errore preciso, per accorgermi subito dell'errore.
18. Come sviluppatore, voglio che eliminare un Field o un Block type non elimini mai i dati salvati, per non perdere contenuti per un errore nel file.
19. Come Client, voglio vedere "Da completare" su un Block che ha un Field obbligatorio nuovo, per sapere cosa compilare.
20. Come Client, voglio vedere i Block di un Block type eliminato come "non più disponibile" e poterli eliminare, per pulire la Page.
21. Come sviluppatore, voglio che un cambio di tipo di un Field con la stessa chiave blocchi l'avvio, per non leggere dati di un tipo sbagliato.

## Implementation Decisions

- Modulo Block type: il registro unisce il set base, che è nel CMS, con il file JSON dell'Installation (ADR 0003). Produce i descrittori dei Field. Lo legge all'avvio e lo valida.
- L'admin costruisce ogni form dai descrittori, con un componente Svelte per tipo di Field (ricerca #5). Non si usa un renderer JSON Schema.
- Tipi di Field: Testo, Testo lungo, Testo formattato, Immagine, Link, Scelta, Lista. Ogni Field può essere obbligatorio. Il Field Immagine è in F5.
- Testo formattato: solo grassetto, corsivo, link, elenchi, H2 e H3. Il CMS rifiuta o pulisce ogni altro tag.
- Traducibili: Testo, Testo lungo, Testo formattato, etichetta del Link. Comuni a tutte le Language: destinazione del Link e Scelta (ADR 0004).
- Set base in questa feature: Testo, Call to action, FAQ. Hero, Testo e immagine, Galleria e Card sono in F5. Form contatti è in F8.
- Contenuto dei Block in una colonna JSON della tabella Block comune (ADR 0003).
- File dei Block type: JSON nel repo del frontend del Site, montato in sola lettura nel container. Il CMS pubblica il suo JSON Schema. Chiavi stabili ed etichette. Un Block type del file non riusa la chiave di un Block type base. Il file può indicare quali Block type base sono attivi. Senza indicazione sono attivi tutti.
- Regole sui cambi (ADR 0005): un Field nuovo è vuoto nei Block salvati. Un Field eliminato resta nel JSON e l'API lo ignora. Un Block type eliminato nasconde i suoi Block dall'API pubblica e li mostra nell'admin come "non più disponibile". La chiave di un Field non si rinomina. Un cambio di tipo di un Field con la stessa chiave è un errore, e il CMS non parte.
- Il contratto OpenAPI dei Block polimorfici è una slice a parte (4.5): la ricerca #5 non ha verificato `JsonDerivedType` con `anyOf`/`oneOf`.

## Testing Decisions

- Seam principale: l'API HTTP con `WebApplicationFactory`, un file SQLite temporaneo e un file dei Block type temporaneo per ogni test.
- Si prova ogni tipo di Field, ogni regola del Field obbligatorio e ogni regola sui cambi di schema.
- Seam di supporto: un test Playwright aggiunge, riordina ed elimina un Block.
- Nessun test unitario dei componenti Svelte.

## Out of Scope

- Block type creati dal Client nell'admin.
- Rinomina della chiave di un Field.
- Field Video, Data, Numero e Colore.
- Mappa Google e video incorporati.

## Further Notes

- Viene dalla spec generale #17 e dalle decisioni #3, #4, #5 e #11.
