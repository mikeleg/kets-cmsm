# 02: Operazioni sui Block nell'editor

**Issue:** [#60](https://github.com/mikeleg/kets-cmsm/issues/60)

**What to build:** Il Client compone una Page aggiungendo, spostando, duplicando ed eliminando Block.

**Blocked by:** F4/01 Registro dei Block type e Testo formattato (#56)

**Status:** ready-for-agent

- [ ] Il Client aggiunge un Block da un elenco di Block type.
- [ ] Un "+" tra due Block inserisce un Block in quel punto.
- [ ] Una barra su ogni Block ha ↑, ↓, duplica ed elimina.
- [ ] Dopo l'eliminazione c'è "Annulla" per 7 secondi, senza finestre di conferma.
- [ ] Tutte le operazioni vanno nella copia di lavoro e si salvano con "Salva".
- [ ] Test sull'API per l'ordine salvato. Un test Playwright aggiunge, riordina ed elimina un Block.
