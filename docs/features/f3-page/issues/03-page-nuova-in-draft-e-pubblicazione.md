# 03: Page nuova in Draft e pubblicazione

**Issue:** [#55](https://github.com/mikeleg/kets-cmsm/issues/55)

**What to build:** Il Client crea una Page nuova, la prepara in Draft e la pubblica quando è pronta. Può anche riportarla in Draft.

**Blocked by:** F3/02 Il Client modifica la Home e salva (#52)

**Status:** ready-for-agent

- [ ] Il Client crea una Page con un titolo. La Page nuova parte in Draft, con il badge "Draft: i visitatori non la vedono".
- [ ] Una Page in Draft risponde 404 all'API pubblica.
- [ ] "Pubblica" salva la Page e la porta in Published con un solo gesto.
- [ ] "Riporta in Draft" c'è su ogni Page Published tranne la Home.
- [ ] Il Client sceglie la Page da modificare da un menu a tendina nell'editor.
- [ ] Test sull'API per gli stati. Un test Playwright crea e pubblica una Page.
