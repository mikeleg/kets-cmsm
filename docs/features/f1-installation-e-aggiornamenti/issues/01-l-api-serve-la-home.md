# 01: L'API serve la Home

**Issue:** [#42](https://github.com/mikeleg/kets-cmsm/issues/42)

**What to build:** Un'Installation parte e il frontend di un Site legge la Home dall'API pubblica.

**Blocked by:** None (can start immediately)

**Status:** ready-for-agent

- [ ] La soluzione .NET Minimal API usa SQLite con EF Core e crea il database al primo avvio.
- [ ] Al primo avvio esiste la Home, Published, con un Block Testo.
- [ ] L'API pubblica restituisce la Home per il percorso `/`.
- [ ] Ogni valore di testo è salvato per Language, es. `{"it": "..."}` (ADR 0004).
- [ ] L'API pubblica accetta `lang`, con `it` come valore predefinito.
- [ ] L'API pubblica ha un contratto OpenAPI.
- [ ] Un test con `WebApplicationFactory` legge la Home attraverso HTTP, su un file SQLite temporaneo.
