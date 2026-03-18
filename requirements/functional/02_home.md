# Fase 2 — Core Features: Home

Overview
--------
La schermata Home è la dashboard principale dell'app, punto di accesso alle azioni rapide (prenota sosta, acquista abbonamento), riepilogo ticket e lista parcheggi vicini. Questo documento descrive il comportamento, gli stati e le regole di business.

User stories
------------
- Come utente voglio vedere i miei ticket attivi e programmati direttamente nella Home.
- Come utente voglio accedere rapidamente alla prenotazione di una sosta o all'acquisto di un abbonamento.
- Come utente voglio vedere una lista di parcheggi vicini basata sulla posizione (se permessi concessi).

Requisiti funzionali
---------------------
1. Dashboard principale
   - Mostra card di azioni rapide: Prenota sosta, Acquista abbonamento.
   - Mostra carosello news e card informative (es. telepedaggio) visibili per utenti loggati.
 
2. Quick actions e card riepilogo
   - Bottoni `btnPrenotaSosta` e `btnAcquistaAbbonamento` con comportamento condizionato dallo stato di autenticazione (apre LoginActivity se non loggato).
   - Sezione ticket: due tab (Active, Scheduled) che filtrano la lista ticket.

3. Navigazione verso altre sezioni
   - Clicking su item parcheggio apre il dettaglio parcheggio (nav_graph). I pulsanti interni navigano a flussi di prenotazione o abbonamento.

