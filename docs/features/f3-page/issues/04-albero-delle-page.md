# 04: Albero delle Page

**Issue:** [#59](https://github.com/mikeleg/kets-cmsm/issues/59)

**What to build:** Il Client organizza le Page ad albero, con URL leggibili che nascono dagli slug.

**Blocked by:** F3/03 Page nuova in Draft e pubblicazione (#55)

**Status:** ready-for-agent

- [ ] Una Page può avere una Page padre, scelta in "Proprietà della Page".
- [ ] L'URL nasce dal percorso degli slug (es. `/servizi/consulenza`). L'API pubblica legge una Page per questo percorso.
- [ ] Lo slug nasce dal titolo e il Client lo può cambiare. Lo slug è un valore per Language.
- [ ] Due Page con lo stesso padre non hanno lo stesso slug.
- [ ] Le figlie di una Page in Draft rispondono 404.
- [ ] Test sull'API per percorsi, slug e regole sulle figlie.
