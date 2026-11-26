# 02: L'admin servito da .NET

**Issue:** [#43](https://github.com/mikeleg/kets-cmsm/issues/43)

**What to build:** Lo sviluppatore apre l'admin SvelteKit dallo stesso dominio dell'API, sia in sviluppo sia nella build di produzione.

**Blocked by:** F1/01 L'API serve la Home (#42)

**Status:** ready-for-agent

- [ ] L'admin SvelteKit si compila con `adapter-static` e lo serve l'API .NET sullo stesso dominio.
- [ ] In sviluppo, lanciare il progetto .NET avvia il dev server di SvelteKit con lo SPA proxy, con hot reload.
- [ ] Una rotta dell'admin che non esiste restituisce l'admin (fallback della SPA), non un 404 dell'API.
- [ ] Un test sull'API controlla che la radice dell'admin restituisca l'HTML dell'admin.
