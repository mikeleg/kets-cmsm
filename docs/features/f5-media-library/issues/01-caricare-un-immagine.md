# 01: Caricare un'immagine

**Issue:** [#53](https://github.com/mikeleg/kets-cmsm/issues/53)

**What to build:** Il Client carica un'immagine e il CMS la salva con le versioni WebP pronte per il Site.

**Blocked by:** F1/04 Immagine Docker con volume (#46); F2/01 Primo Account da comando, login e logout (#47)

**Status:** ready-for-agent

- [ ] Verifica nell'immagine Docker Linux la codifica WebP di SkiaSharp e la licenza della sua parte nativa. Se la verifica fallisce, usa NetVips e scrivi perché.
- [ ] Il CMS accetta JPEG, PNG e WebP, fino a 10 MB e 6000 pixel sul lato lungo. Gli altri file hanno un messaggio chiaro.
- [ ] Al caricamento il CMS genera versioni WebP larghe 480, 960, 1600 e 2400 pixel, senza mai ingrandire.
- [ ] Ogni file ha un URL con un hash del contenuto e `Cache-Control: immutable`.
- [ ] I file stanno nel volume dell'Installation.
- [ ] Test sull'API con file veri: formati, limiti e versioni.
