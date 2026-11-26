# 05: Immagine su GHCR con versione semver

**Issue:** [#48](https://github.com/mikeleg/kets-cmsm/issues/48)

**What to build:** Lo sviluppatore pubblica una versione del CMS che ogni Installation può fissare.

**Blocked by:** F1/04 Immagine Docker con volume (#46)

**Status:** ready-for-agent

- [ ] Una pipeline pubblica l'immagine Docker su GitHub Container Registry con una versione semver, a partire da un tag git.
- [ ] Le Installation non usano il tag `latest`.
- [ ] La versione del CMS è visibile in una risposta dell'API.
