# 04: Immagine Docker con volume

**Issue:** [#46](https://github.com/mikeleg/kets-cmsm/issues/46)

**What to build:** Lo sviluppatore avvia un'Installation con un solo container e un volume.

**Blocked by:** F1/02 L'admin servito da .NET (#43)

**Status:** ready-for-agent

- [ ] Un Dockerfile multi-stage produce un'immagine con API e admin.
- [ ] Il database SQLite e i media stanno in un volume. Il percorso del volume è una variabile d'ambiente.
- [ ] Un container nuovo con un volume vuoto parte e serve la Home.
- [ ] Un container sostituito con lo stesso volume ritrova i dati.
- [ ] Un test Playwright apre l'admin servito dall'immagine Docker.
