# 03: Copia di SQLite e migrazioni all'avvio

**Issue:** [#44](https://github.com/mikeleg/kets-cmsm/issues/44)

**What to build:** A ogni avvio l'API mette al sicuro il database prima di cambiarlo, e non parte con un database a metà.

**Blocked by:** F1/01 L'API serve la Home (#42)

**Status:** ready-for-agent

- [ ] All'avvio l'API copia il file SQLite, poi applica le migrazioni EF Core.
- [ ] Il CMS tiene solo le ultime 3 copie.
- [ ] Se una migrazione fallisce, l'API non parte e il log indica la migrazione.
- [ ] Test sull'API: avvio con una migrazione nuova, avvio con una migrazione che fallisce, rotazione delle copie.
