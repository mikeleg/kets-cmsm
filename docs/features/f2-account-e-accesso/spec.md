# Spec F2: Account e accesso

**Issue:** [#35](https://github.com/mikeleg/kets-cmsm/issues/35)

## Problem Statement

Il Client deve entrare nell'admin del suo Site in modo sicuro, senza password predefinite e senza chiamare lo sviluppatore per ogni collega. Il CMS non manda email nell'MVP, quindi inviti e reset della password non possono passare per la posta.

## Solution

Lo sviluppatore crea il primo Account con un comando nel container, che stampa un link monouso per scegliere la password. Il Client entra con email e password. Ogni Account crea o elimina altri Account dall'admin, con un link monouso da mandare a mano. Per una password dimenticata lo sviluppatore usa un comando che stampa un link di reset. C'è un solo ruolo.

## User Stories

1. Come sviluppatore, voglio creare il primo Account di un'Installation con un comando nel container, per non avere mai una password predefinita.
2. Come sviluppatore, voglio che il comando stampi un link monouso per scegliere la password, per consegnarlo al Client.
3. Come Client, voglio scegliere la mia password dal link, per essere l'unico a conoscerla.
4. Come Client, voglio una password di almeno 12 caratteri senza regole di complessità, per sceglierne una lunga e facile da ricordare.
5. Come Client, voglio entrare nell'admin con email e password, per gestire i contenuti del mio Site.
6. Come Client, voglio uscire dall'admin, per lasciare un computer condiviso senza rischi.
7. Come Client, voglio restare connesso per 14 giorni se uso l'admin, per non rifare l'accesso ogni volta.
8. Come Client, voglio che il mio Account si blocchi per 15 minuti dopo 5 tentativi sbagliati, per proteggerlo da chi prova le password.
9. Come Client, voglio creare un Account per un collega dall'admin e ricevere un link monouso da mandargli, per non dover chiamare lo sviluppatore.
10. Come Client, voglio che il link per un nuovo Account scada dopo 48 ore, per non lasciare inviti aperti.
11. Come Client, voglio eliminare un Account, ma non l'ultimo rimasto, per non restare fuori dall'admin.
12. Come sviluppatore, voglio un comando nel container che genera un link di reset della password valido 1 ora, per aiutare un Client che l'ha dimenticata.
13. Come sviluppatore, voglio che l'API dell'admin risponda 401 senza sessione, per proteggere ogni dato dell'admin.

## Implementation Decisions

- ASP.NET Core Identity su SQLite, con un cookie di sessione e senza JWT.
- Cookie `HttpOnly`, `Secure`, `SameSite=Strict`, di 14 giorni rinnovati con l'uso. L'admin sta sullo stesso dominio dell'API.
- Password di almeno 12 caratteri, senza obbligo di complessità. Blocco di 15 minuti dopo 5 tentativi sbagliati.
- Comandi nel container: `cms create-account <email>` stampa un link monouso di 48 ore. `cms reset-password <email>` stampa un link monouso di 1 ora.
- Link per creare un Account dall'admin: monouso, validi 48 ore, mostrati nell'admin e mandati a mano.
- Un solo ruolo: tutti gli Account hanno gli stessi diritti.
- Il CMS non manda email e non ha una configurazione SMTP.

## Testing Decisions

- Test in-process sull'API HTTP con `WebApplicationFactory` e un file SQLite temporaneo. Si controllano solo risposte HTTP e cookie, non le tabelle di Identity.
- I comandi del container si provano chiamando lo stesso punto di ingresso del container nel test.
- Un test Playwright fa il login nell'admin.

## Out of Scope

- Autenticazione a due fattori.
- Email di invito e di reset. Configurazione SMTP.
- Ruoli diversi o permessi per Account.
- Reset della password dall'admin.

## Further Notes

- Viene dalla spec generale #17 e dalla decisione #13 (con la revisione "nessuna email nell'MVP").
- Rischio da dire al Client: per una password dimenticata serve lo sviluppatore.
