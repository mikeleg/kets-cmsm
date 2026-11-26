# 04: Conservazione per 12 mesi

**Issue:** [#76](https://github.com/mikeleg/kets-cmsm/issues/76)

**What to build:** Il CMS rispetta il GDPR senza che il Client ci pensi.

**Blocked by:** F8/01 Ricevere una Contact request (#66)

**Status:** ready-for-agent

- [ ] Il CMS elimina da solo le Contact request più vecchie di 12 mesi.
- [ ] L'eliminazione gira in background, senza un'azione del Client.
- [ ] Test sull'API con un orologio controllato dal test.
