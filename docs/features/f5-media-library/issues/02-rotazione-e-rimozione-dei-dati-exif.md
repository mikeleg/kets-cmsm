# 02: Rotazione e rimozione dei dati EXIF

**Issue:** [#57](https://github.com/mikeleg/kets-cmsm/issues/57)

**What to build:** Il Client carica le foto del telefono senza pubblicare dati privati e senza foto ruotate.

**Blocked by:** F5/01 Caricare un'immagine (#53)

**Status:** ready-for-agent

- [ ] Il CMS ruota la foto nel verso giusto secondo l'orientamento EXIF.
- [ ] Il CMS toglie tutti i metadati EXIF, compresa la posizione GPS, dall'originale e dalle versioni.
- [ ] Il CMS tiene l'originale ripulito.
- [ ] Test sull'API con una foto vera ruotata e con dati GPS.
