# 05: Spostare una Page e cambiare lo slug con redirect 301

**Issue:** [#67](https://github.com/mikeleg/kets-cmsm/issues/67)

**What to build:** Il Client cambia slug o posizione di una Page senza rompere i link già esistenti.

**Blocked by:** F3/04 Albero delle Page (#59)

**Status:** ready-for-agent

- [ ] Spostare una Page vale subito, fuori da "Salva", dopo un avviso: "cambia l'URL di questa Page e delle sue figlie".
- [ ] La Home sta sempre a `/` e non si sposta. Una Page non si sposta dentro se stessa o dentro una sua figlia.
- [ ] Ogni cambio di slug o di posizione di una Page Published crea un redirect 301 per la Page e per le sue figlie.
- [ ] L'API pubblica restituisce il redirect 301 sul vecchio percorso.
- [ ] Test sull'API per i redirect, anche dopo due cambi di fila, e per gli spostamenti vietati.
