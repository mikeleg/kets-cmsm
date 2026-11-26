# 02: Il Client modifica la Home e salva

**Issue:** [#52](https://github.com/mikeleg/kets-cmsm/issues/52)

**What to build:** Il Client apre la Home nell'editor visivo, modifica il Block Testo e salva. La modifica va online subito.

**Blocked by:** F2/01 Primo Account da comando, login e logout (#47)

**Status:** ready-for-agent

- [ ] L'editor mostra la Page disegnata al centro. Il clic su un Block apre il suo form in un pannello laterale (variante B del prototipo dell'editor).
- [ ] Le modifiche vanno in una copia di lavoro e l'admin mostra "Modifiche non salvate".
- [ ] Un solo "Salva" salva tutta la Page. Su una Page Published mostra "Online subito".
- [ ] Dopo il salvataggio, la prossima lettura pubblica restituisce il contenuto nuovo.
- [ ] Il testo salvato resta un valore per Language.
- [ ] Test sull'API per il salvataggio. Un test Playwright modifica e salva la Home.
