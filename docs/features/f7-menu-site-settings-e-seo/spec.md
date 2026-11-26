# Spec F7: Menu, Site settings e SEO

**Issue:** [#40](https://github.com/mikeleg/kets-cmsm/issues/40)

## Problem Statement

Il Client deve decidere la navigazione del suo Site e aggiornare i dati che compaiono su ogni Page, come il logo e i contatti. Ogni Page deve apparire bene su Google e sui social senza che il Client ripeta gli stessi testi. Un link verso una Page non pronta o eliminata non deve mai comparire.

## Solution

Ogni Site ha due Menu fissi, principale e footer. Ogni voce punta a una Page o a un URL esterno e il Client ne decide l'ordine. Le Site settings tengono nome, logo, contatti, social e SEO predefinito. Ogni Page ha title, description e immagine Open Graph, e un campo vuoto prende il valore delle Site settings. L'API pubblica restituisce Menu, Site settings e una sitemap senza Page in Draft.

## User Stories

1. Come Client, voglio gestire il Menu principale e il Menu footer, per decidere la navigazione del Site.
2. Come Client, voglio che una voce di Menu punti a una Page o a un URL esterno, per collegare anche siti esterni.
3. Come Client, voglio scrivere l'etichetta di ogni voce, per usare un testo più corto del titolo della Page.
4. Come Client, voglio riordinare le voci di un Menu, per decidere l'ordine della navigazione.
5. Come visitatore, voglio che una voce di Menu verso una Page in Draft o eliminata non compaia, per non trovare link rotti.
6. Come Client, voglio vedere nella Preview la voce di Menu della Page in Draft, per vedere il Site come sarà dopo la pubblicazione.
7. Come Client, voglio modificare le Site settings (nome, logo, contatti, social, SEO predefinito), per aggiornare i dati che compaiono su ogni Page.
8. Come Client, voglio compilare title, description e immagine Open Graph di ogni Page, per come la Page appare su Google e sui social.
9. Come Client, voglio che un campo SEO vuoto prenda il valore predefinito delle Site settings, per non doverlo ripetere su ogni Page.
10. Come visitatore, voglio che una Page in Draft non sia nella sitemap, per non trovare contenuti non pronti nei motori di ricerca.
11. Come sviluppatore, voglio che le Site settings abbiano l'elenco delle Language, per aggiungere una Language dopo senza migrare i dati.

## Implementation Decisions

- Modulo Contenuto: Menu e Site settings.
- Due Menu fissi per Site: principale e footer. Una voce ha un'etichetta e una destinazione: una Page o un URL esterno.
- L'API pubblica non restituisce le voci verso una Page in Draft o eliminata. In Preview, la voce della Page in Preview compare anche se la Page è in Draft.
- Site settings con campi fissi: nome, logo (Field Immagine), contatti, social, valori SEO predefiniti.
- Le Site settings hanno l'elenco delle Language e quella predefinita, fisse a `it` nell'MVP e non visibili al Client.
- SEO per Page: title, description e immagine Open Graph, in "Proprietà della Page". Un campo vuoto prende il valore predefinito delle Site settings.
- Traduzione: etichette dei Menu, campi SEO e testi delle Site settings sono valori per Language, come gli altri testi (ADR 0004, decisione #9).
- L'API pubblica restituisce Menu, Site settings e sitemap.

## Testing Decisions

- Seam principale: l'API HTTP con `WebApplicationFactory`. Copre visibilità delle voci di Menu, voce in Preview, valori SEO predefiniti e sitemap.
- Nessun test unitario dei componenti Svelte.

## Out of Scope

- Menu oltre ai due fissi e sottomenu.
- SEO avanzata oltre a title, description e immagine Open Graph.
- Language diverse dall'italiano.

## Further Notes

- Viene dalla spec generale #17 e dalle decisioni #2, #8 e #9.
- La frase dell'ADR 0004 su "same value for each Language" è ambigua. Questa spec legge le etichette dei Menu, i campi SEO e i testi delle Site settings come valori per Language, come la spec #17. Se la lettura è sbagliata, correggi l'ADR o questa spec.
