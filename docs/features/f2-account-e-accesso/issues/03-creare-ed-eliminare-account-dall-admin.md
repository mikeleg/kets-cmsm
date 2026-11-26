# 03: Creare ed eliminare Account dall'admin

**Issue:** [#50](https://github.com/mikeleg/kets-cmsm/issues/50)

**What to build:** Il Client aggiunge un collega e toglie un Account che non serve più, senza chiamare lo sviluppatore.

**Blocked by:** F2/01 Primo Account da comando, login e logout (#47)

**Status:** ready-for-agent

- [ ] Un Account crea un altro Account dall'admin con un'email. L'admin mostra un link monouso, valido 48 ore, da mandare a mano.
- [ ] Un link usato o scaduto non funziona più.
- [ ] Un Account elimina un altro Account, ma non l'ultimo rimasto.
- [ ] C'è un solo ruolo.
- [ ] Test sull'API per creazione, scadenza dei link e ultimo Account.
