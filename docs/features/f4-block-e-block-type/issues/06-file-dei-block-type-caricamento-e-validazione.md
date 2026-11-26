# 06: File dei Block type: caricamento e validazione

**Issue:** [#64](https://github.com/mikeleg/kets-cmsm/issues/64)

**What to build:** Lo sviluppatore aggiunge un Block type per un solo Site in un file JSON, e un file sbagliato blocca l'avvio con un errore preciso (ADR 0003).

**Blocked by:** F4/01 Registro dei Block type e Testo formattato (#56)

**Status:** ready-for-agent

- [ ] Il CMS legge all'avvio un file JSON montato in sola lettura nel container. Il CMS pubblica il JSON Schema del file.
- [ ] Ogni Block type e ogni Field hanno una chiave stabile e un'etichetta. Un Block type del file non riusa la chiave di un Block type base.
- [ ] Il file indica quali Block type base sono attivi. Senza indicazione sono attivi tutti.
- [ ] Un file non valido blocca l'avvio con un errore preciso.
- [ ] Un Block type del file compare nell'elenco dei Block type dell'editor e ha il suo form.
- [ ] Test sull'API per ogni regola sopra, con un file dei Block type temporaneo per ogni test.
