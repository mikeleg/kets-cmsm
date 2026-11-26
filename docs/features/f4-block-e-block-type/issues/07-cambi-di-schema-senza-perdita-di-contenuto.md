# 07: Cambi di schema senza perdita di contenuto

**Issue:** [#69](https://github.com/mikeleg/kets-cmsm/issues/69)

**What to build:** Lo sviluppatore cambia il file dei Block type e nessun contenuto salvato si perde (ADR 0005).

**Blocked by:** F4/06 File dei Block type: caricamento e validazione (#64)

**Status:** ready-for-agent

- [ ] Un Field nuovo è vuoto nei Block salvati. Se è obbligatorio, l'admin segna il Block "Da completare" e la Page resta online.
- [ ] Un Field eliminato resta nel JSON salvato e l'API lo ignora. Torna se il Field torna con la stessa chiave.
- [ ] Un Block type eliminato nasconde i suoi Block dall'API pubblica. L'admin li mostra come "Block type non più disponibile" e il Client li può eliminare. Il log indica le Page che li contengono.
- [ ] Un cambio di tipo di un Field con la stessa chiave blocca l'avvio.
- [ ] Il cambio dell'etichetta di un Field non tocca i dati.
- [ ] Test sull'API per ogni regola sopra.
