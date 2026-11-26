# 04: Field Immagine, Hero e Testo e immagine

**Issue:** [#70](https://github.com/mikeleg/kets-cmsm/issues/70)

**What to build:** Il Client usa le immagini della Media library nei Block type Hero e Testo e immagine.

**Blocked by:** F5/03 Media library nell'admin (#58); F4/03 Link e Scelta, Block type Call to action (#61)

**Status:** ready-for-agent

- [ ] Il Field Immagine sceglie un'immagine dalla Media library e chiede il testo alternativo, che è obbligatorio e traducibile.
- [ ] Il file dell'immagine è comune a tutte le Language (ADR 0004).
- [ ] Funzionano i Block type Hero (titolo, sottotitolo, immagine, link CTA) e Testo e immagine (titolo, testo formattato, immagine, Scelta della posizione).
- [ ] L'API pubblica restituisce per ogni immagine le versioni WebP pronte per `srcset`.
- [ ] Test sull'API per il Field Immagine e per i due Block type.
