# Spec F5: Media library

**Issue:** [#38](https://github.com/mikeleg/kets-cmsm/issues/38)

## Problem Statement

Il Client vuole usare le sue foto nel Site. Le foto arrivano dal telefono: sono pesanti, a volte ruotate, e contengono la posizione GPS. Il visitatore deve ricevere immagini leggere nella dimensione giusta, e il Client non deve poter rompere un'immagine in uso.

## Solution

Il Client carica JPEG, PNG e WebP nella Media library. Il CMS ruota la foto, toglie i metadati EXIF e genera versioni WebP in più larghezze. Ogni file ha un URL con un hash del contenuto, che non cambia mai. Il Client usa le immagini nei Block con il Field Immagine e un testo alternativo obbligatorio. Un'immagine in uso non si elimina.

## User Stories

1. Come Client, voglio caricare immagini JPEG, PNG e WebP fino a 10 MB, per usarle nei Block.
2. Come Client, voglio un messaggio chiaro quando un file non è accettato, per sapere cosa caricare.
3. Come visitatore, voglio immagini WebP nella dimensione giusta per il mio schermo, per caricare il Site in fretta.
4. Come visitatore, voglio che un'immagine sostituita non mi venga mostrata vecchia dalla cache, per vedere sempre il contenuto giusto.
5. Come Client, voglio che una foto scattata in verticale resti in verticale, per non doverla ruotare a mano.
6. Come Client, voglio che il CMS tolga la posizione GPS e gli altri metadati dalle foto, per non pubblicare dati privati.
7. Come Client, voglio vedere tutte le immagini in un elenco con le anteprime, per scegliere quella giusta.
8. Come Client, voglio cercare un'immagine per nome nella Media library, per ritrovarla in fretta.
9. Come Client, voglio scegliere un'immagine dalla Media library e scriverne il testo alternativo, per un Site accessibile.
10. Come Client, voglio usare i Block type Hero e Testo e immagine, per presentare la mia attività.
11. Come Client, voglio decidere se l'immagine sta a sinistra o a destra del testo, per variare il layout.
12. Come Client, voglio una Galleria e una lista di Card, per mostrare lavori e servizi.
13. Come sviluppatore del frontend, voglio le versioni di ogni immagine pronte per `srcset`, per non calcolarle io.
14. Come Client, voglio sapere in quali Page è usata un'immagine e non poterla eliminare finché è in uso, per non avere immagini rotte sul Site.

## Implementation Decisions

- Modulo Media library: caricamento, validazione, ripulitura dai metadati e generazione delle versioni con SkiaSharp.
- La prima slice verifica nell'immagine Docker Linux la codifica WebP di SkiaSharp e la licenza della sua parte nativa. Se la verifica fallisce, si usa NetVips (ricerca #6, decisione #15).
- Formati JPEG, PNG e WebP. Al massimo 10 MB e 6000 pixel sul lato lungo.
- Il CMS ruota la foto nel verso giusto, poi toglie i dati EXIF. Tiene l'originale ripulito.
- Versioni WebP larghe 480, 960, 1600 e 2400 pixel, generate al caricamento, senza mai ingrandire.
- Ogni file ha un URL con l'hash del contenuto e `Cache-Control: immutable`. Un'immagine sostituita ha un URL nuovo.
- I file stanno nel volume dell'Installation.
- Field Immagine: un riferimento a un'immagine della Media library e un testo alternativo obbligatorio e traducibile. Il file è comune a tutte le Language (ADR 0004).
- Block type del set base in questa feature: Hero (titolo, sottotitolo, immagine, link CTA), Testo e immagine (titolo, testo formattato, immagine, posizione dell'immagine), Galleria (lista di immagini), Card (lista di card con immagine, titolo, testo, link).
- Un'immagine in uso non si elimina. L'admin mostra le Page che la usano.

## Testing Decisions

- Seam principale: l'API HTTP con `WebApplicationFactory`, con file veri: formati ammessi e no, limiti, EXIF, versioni generate, immagine in uso.
- La verifica di SkiaSharp gira nell'immagine Docker Linux, non solo sulla macchina di sviluppo.
- Nessun test unitario dei componenti Svelte.

## Out of Scope

- Upload di PDF e altri file. Il Client usa un Link a un URL esterno.
- Immagini SVG, GIF e HEIC.
- Cartelle nella Media library.
- Ritaglio e modifica delle immagini nell'admin.
- CDN.

## Further Notes

- Viene dalla spec generale #17 e dalle decisioni #6, #12 e #15.
