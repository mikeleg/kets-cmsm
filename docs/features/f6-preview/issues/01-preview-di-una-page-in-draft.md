# 01: Preview di una Page in Draft

**Issue:** [#65](https://github.com/mikeleg/kets-cmsm/issues/65)

**What to build:** Il Client apre la Preview di una Page in Draft sul frontend del Site.

**Blocked by:** F3/03 Page nuova in Draft e pubblicazione (#55)

**Status:** ready-for-agent

- [ ] L'admin invia la copia di lavoro della Page all'API. L'API la tiene in memoria sotto un token di 30 minuti.
- [ ] L'API pubblica con `?preview=<token>` restituisce la copia di lavoro, senza cache.
- [ ] Un token scaduto o sconosciuto ha una risposta dedicata.
- [ ] Il pulsante Anteprima nell'admin apre il frontend del Site con il token.
- [ ] Un token non cambia mai quello che vedono i visitatori: la Page in Draft risponde ancora 404 senza token.
- [ ] Test sull'API per token, scadenza e nessun effetto sui visitatori.
