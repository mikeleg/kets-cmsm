# 01: Ricevere una Contact request

**Issue:** [#66](https://github.com/mikeleg/kets-cmsm/issues/66)

**What to build:** Un visitatore manda una Contact request dal Form contatti del Site e il CMS la salva.

**Blocked by:** F4/01 Registro dei Block type e Testo formattato (#56)

**Status:** ready-for-agent

- [ ] Il Block type Form contatti ha titolo e testo introduttivo. I campi del form sono fissi: nome, email, telefono facoltativo, messaggio, consenso privacy obbligatorio.
- [ ] Il browser del visitatore manda la Contact request all'API pubblica. Il CORS accetta solo il dominio del Site.
- [ ] Un invio senza un campo obbligatorio o senza consenso ha un errore chiaro.
- [ ] Il CMS non manda email.
- [ ] Test sull'API per invio, campi obbligatori e CORS.
