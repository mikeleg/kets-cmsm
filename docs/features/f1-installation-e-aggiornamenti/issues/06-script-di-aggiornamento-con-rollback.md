# 06: Script di aggiornamento con rollback

**Issue:** [#54](https://github.com/mikeleg/kets-cmsm/issues/54)

**What to build:** Lo sviluppatore aggiorna le Installation una alla volta e torna indietro da solo se qualcosa fallisce.

**Blocked by:** F1/03 Copia di SQLite e migrazioni all'avvio (#44); F1/05 Immagine su GHCR con versione semver (#48)

**Status:** ready-for-agent

- [ ] Uno script aggiorna un elenco di Installation alla versione indicata, una alla volta.
- [ ] Lo script si ferma al primo errore e segnala quale Installation ha fallito e perché.
- [ ] Se una migrazione fallisce, lo script ripristina la copia del file SQLite e rimette la versione precedente dell'immagine.
- [ ] Basta un host che fa girare un container Docker con un volume persistente.
- [ ] Una prova dello script con una migrazione che fallisce apposta.
