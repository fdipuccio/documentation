#— Core Features: Autenticazione

Overview
--------
Questo documento descrive la funzionalità di autenticazione dell'app FS PARK (Fase 2). Copre i percorsi di Login via WebView, Registrazione completa (form con nation picker e consensi), Reset password, stati, validazioni e la gestione degli errori.

User stories
------------
- Come utente voglio poter effettuare il login tramite WebView per autenticare il mio account e accedere alle funzionalità protette.
- Come nuovo utente voglio poter completare la registrazione inserendo nome, cognome, email, password, paese di nascita e codice fiscale, e accettare i consensi richiesti.
- Come utente registrato voglio poter resettare la password quando la dimentico.

Requisiti funzionali
---------------------
1. Login via WebView
   - L'app apre una WebView che punta all'endpoint di autorizzazione esterno; la WebView intercetta i redirect per ottenere il code di autorizzazione e completare il flusso OAuth.
   
2. Registrazione completa
   - Form di registrazione con i campi: nome, cognome, email, password, ripeti password, nazione (nation picker con flag), codice fiscale (CF).


Business rules
--------------

UI Components
-------------
- Login 
- Registrazione
Validazioni
-----------
- Nome / Cognome: caratteri alfabetici, prima lettera maiuscola (pulizia implementata in `setupField()`).
- Email: formato valido secondo `Patterns.EMAIL_ADDRESS`.
- Password: forza minima come `isPasswordStrong()`.
- Ripeti password: deve corrispondere alla password.
- Codice fiscale: per ISO IT verifica con regex `^[A-Za-z]{6}[0-9]{2}[A-Za-z][0-9]{2}[A-Za-z][0-9]{3}[A-Za-z]$` in 
- Consenso condizioni: obbligatorio.


