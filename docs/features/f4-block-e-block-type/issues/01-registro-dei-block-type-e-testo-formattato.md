# 01: Registro dei Block type e Testo formattato

**Issue:** [#56](https://github.com/mikeleg/kets-cmsm/issues/56)

**What to build:** Il Client modifica il Block Testo con un form costruito dal registro dei Block type, e non può salvare un Field obbligatorio vuoto.

**Blocked by:** F3/02 Il Client modifica la Home e salva (#52)

**Status:** ready-for-agent

- [ ] Il registro dei Block type contiene il set base e produce i descrittori dei Field.
- [ ] L'admin costruisce il form di un Block dai descrittori, con un componente Svelte per tipo di Field.
- [ ] Il Field Testo e il Field Testo formattato funzionano. Il Testo formattato accetta solo grassetto, corsivo, link, elenchi, H2 e H3.
- [ ] Il Block type Testo usa il registro.
- [ ] Un Field obbligatorio vuoto blocca "Salva" e apre il Block con l'errore.
- [ ] Test sull'API per i descrittori, per il Testo formattato con tag non ammessi e per il Field obbligatorio.
