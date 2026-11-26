# 02: Invio automatico e un token alla volta

**Issue:** [#71](https://github.com/mikeleg/kets-cmsm/issues/71)

**What to build:** La Preview mostra sempre l'ultima modifica del Client, senza pulsanti in più.

**Blocked by:** F6/01 Preview di una Page in Draft (#65)

**Status:** ready-for-agent

- [ ] L'admin invia la copia di lavoro da solo dopo ogni modifica, con un breve ritardo.
- [ ] Ogni invio rinnova la scadenza del token.
- [ ] Un token vale solo per la sua Page. Aprire un'altra Page nell'admin chiude il token precedente.
- [ ] Test sull'API per rinnovo e chiusura del token.
