# 02: Blocco dopo 5 tentativi

**Issue:** [#49](https://github.com/mikeleg/kets-cmsm/issues/49)

**What to build:** Un Account resiste a chi prova le password.

**Blocked by:** F2/01 Primo Account da comando, login e logout (#47)

**Status:** ready-for-agent

- [ ] Dopo 5 tentativi sbagliati l'Account si blocca per 15 minuti.
- [ ] Durante il blocco anche la password giusta non entra, e l'admin mostra un messaggio chiaro.
- [ ] Un login riuscito azzera il conteggio dei tentativi.
- [ ] Test sull'API per blocco, durata e azzeramento.
