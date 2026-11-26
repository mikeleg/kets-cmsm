# 01: Cache ed ETag sulle letture pubbliche

**Issue:** [#45](https://github.com/mikeleg/kets-cmsm/issues/45)

**What to build:** Il frontend chiama l'API a ogni richiesta senza pesare sul CMS, e vede ogni salvataggio dalla richiesta dopo.

**Blocked by:** F1/01 L'API serve la Home (#42)

**Status:** ready-for-agent

- [ ] L'API tiene le risposte pubbliche in una cache in memoria.
- [ ] Ogni salvataggio svuota la cache.
- [ ] Le letture pubbliche hanno un `ETag` e rispondono `304 Not Modified` alle richieste condizionali.
- [ ] Test sull'API: risposta dalla cache, cache svuotata dopo un salvataggio, `304` con lo stesso `ETag`, `200` dopo un salvataggio.
