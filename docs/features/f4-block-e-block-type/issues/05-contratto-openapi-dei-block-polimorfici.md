# 05: Contratto OpenAPI dei Block polimorfici

**Issue:** [#63](https://github.com/mikeleg/kets-cmsm/issues/63)

**What to build:** Lo sviluppatore del frontend genera un client tipizzato in cui ogni Block type ha i suoi Field.

**Blocked by:** F4/01 Registro dei Block type e Testo formattato (#56)

**Status:** ready-for-agent

- [ ] Il contratto OpenAPI descrive i Block con Field diversi per Block type, con un discriminatore sul Block type.
- [ ] Verifica il comportamento di `JsonDerivedType` con `anyOf`/`oneOf` e scrivi la soluzione scelta in un ADR.
- [ ] I Block type del file dell'Installation compaiono nel contratto, oppure l'ADR spiega come il frontend li tipizza.
- [ ] Un test controlla che il contratto generato contenga ogni Block type del set base.
