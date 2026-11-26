# 01: Primo Account da comando, login e logout

**Issue:** [#47](https://github.com/mikeleg/kets-cmsm/issues/47)

**What to build:** Lo sviluppatore crea il primo Account con un comando nel container. Il Client sceglie la password ed entra nell'admin.

**Blocked by:** F1/02 L'admin servito da .NET (#43)

**Status:** ready-for-agent

- [ ] Il comando `cms create-account <email>` nel container stampa un link monouso, valido 48 ore, per scegliere la password. Non esiste una password predefinita.
- [ ] La password ha almeno 12 caratteri, senza obbligo di complessità.
- [ ] Login con email e password e logout funzionano.
- [ ] La sessione è un cookie `HttpOnly`, `Secure`, `SameSite=Strict`, di 14 giorni rinnovati con l'uso (ASP.NET Core Identity su SQLite).
- [ ] L'API dell'admin risponde 401 senza sessione.
- [ ] Test sull'API per comando, link scaduto, login, logout e 401. Un test Playwright fa il login.
