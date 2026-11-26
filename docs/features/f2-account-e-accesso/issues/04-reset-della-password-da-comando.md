# 04: Reset della password da comando

**Issue:** [#51](https://github.com/mikeleg/kets-cmsm/issues/51)

**What to build:** Lo sviluppatore aiuta un Client che ha dimenticato la password.

**Blocked by:** F2/01 Primo Account da comando, login e logout (#47)

**Status:** ready-for-agent

- [ ] Il comando `cms reset-password <email>` nel container stampa un link monouso, valido 1 ora.
- [ ] Il link permette di scegliere una password nuova con le stesse regole.
- [ ] Un'email senza Account dà un errore chiaro nel comando.
- [ ] Test per il comando, il link e la scadenza.
