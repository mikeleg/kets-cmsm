# Spec F8: Form contatti

**Issue:** [#41](https://github.com/mikeleg/kets-cmsm/issues/41)

## Problem Statement

Un visitatore vuole contattare il Client dal Site. Il Client deve leggere i messaggi veri senza spam, e il CMS deve rispettare il GDPR senza che il Client ci pensi. Il CMS non manda email nell'MVP.

## Solution

Il Block type Form contatti mostra un form con campi fissi. Il browser del visitatore manda la Contact request direttamente all'API pubblica, con il CORS limitato al dominio del Site. Il CMS ferma lo spam con un campo nascosto, un tempo minimo e un limite per IP. Il Client legge le Contact request nell'admin. Il CMS le elimina da solo dopo 12 mesi.

## User Stories

1. Come Client, voglio aggiungere un Form contatti a una Page, con un titolo e un testo introduttivo, per farmi contattare dai visitatori.
2. Come visitatore, voglio mandare una Contact request dal Site con nome, email, telefono facoltativo e messaggio, per contattare il Client.
3. Come visitatore, voglio dare il consenso privacy nel form, per sapere come useranno i miei dati.
4. Come Client, voglio che solo il mio Site possa mandare Contact request al CMS, per non ricevere invii da altri siti.
5. Come Client, voglio che i bot non riempiano le Contact request di spam, per leggere solo i contatti veri.
6. Come Client, voglio leggere le Contact request nell'admin, per gestire i contatti.
7. Come Client, voglio segnare una Contact request come letta, per sapere cosa ho già visto.
8. Come Client, voglio eliminare una Contact request, per togliere quelle che non servono.
9. Come Client, voglio vedere quante Contact request non lette ho appena entro nell'admin, per non perdere un contatto.
10. Come Client, voglio che le Contact request più vecchie di 12 mesi si eliminino da sole, per rispettare il GDPR senza pensarci.

## Implementation Decisions

- Modulo Contact request: ricezione pubblica con difese contro lo spam, elenco e stato letto/non letto nell'admin, eliminazione automatica dopo 12 mesi.
- Il Block type Form contatti ha titolo e testo introduttivo. I campi del form sono fissi: nome, email, telefono facoltativo, messaggio, consenso privacy obbligatorio.
- Il browser del visitatore manda la Contact request all'API pubblica. Il CORS accetta solo il dominio del Site. Così il CMS vede l'IP vero del visitatore.
- Spam: campo nascosto (honeypot), tempo minimo di compilazione di 3 secondi, al massimo 5 invii all'ora per IP.
- Il CMS non manda email e non ha una configurazione SMTP.
- Conservazione fissa di 12 mesi.

## Testing Decisions

- Seam principale: l'API HTTP con `WebApplicationFactory`. Copre invio, campi obbligatori, CORS, ogni difesa contro lo spam e la conservazione.
- La conservazione si prova con un orologio controllato dal test.
- Nessun test unitario dei componenti Svelte.

## Out of Scope

- Email di notifica al Client e email di conferma al visitatore.
- Captcha di terze parti.
- Esportazione CSV delle Contact request.
- Campi del form scelti dal Client.

## Further Notes

- Viene dalla spec generale #17 e dalla decisione #14 (con la revisione "nessuna email nell'MVP").
- Rischio da dire al Client: se non entra nell'admin, non sa che ha nuove Contact request.
