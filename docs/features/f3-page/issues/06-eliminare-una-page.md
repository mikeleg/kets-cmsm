# 06: Eliminare una Page

**Issue:** [#68](https://github.com/mikeleg/kets-cmsm/issues/68)

**What to build:** Il Client elimina una Page che non serve più, senza poter cancellare per errore un ramo intero.

**Blocked by:** F3/04 Albero delle Page (#59)

**Status:** ready-for-agent

- [ ] Si elimina solo una Page senza figlie, e mai la Home.
- [ ] Prima dell'eliminazione c'è una conferma dentro la pagina, senza finestre del browser.
- [ ] L'URL di una Page eliminata risponde 410.
- [ ] Test sull'API per le regole sopra.
