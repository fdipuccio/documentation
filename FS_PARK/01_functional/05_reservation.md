# Fase 2 — Core Features: Reservation (Prenotazione Parcheggio)

Overview
--------
Questo documento descrive il flusso completo di prenotazione della sosta: selezione parcheggio, scelta data/ora, selezione targa, calcolo prezzo, conferma e pagamento. Si basa sui fragment `PurchaseReservationFragment`, `PlatesReservationFragment` e mock API per prezzi.

User stories
------------
- Come utente voglio selezionare un parcheggio e definire check-in/check-out per prenotare una sosta.
- Come utente voglio selezionare una targa esistente o aggiungerne una nuova prima dell'acquisto.
- Come utente voglio vedere il prezzo stimato prima della conferma e procedere al pagamento.
- Come utente voglio poter modificare una prenotazione esistente (extend/reduce) dal dettaglio ticket.

Requisiti funzionali
---------------------
1. Flusso di prenotazione
   - L'utente seleziona un parcheggio (da Home o ricerca) e viene guidato attraverso step: dettaglio parcheggio -> selezione targa -> riepilogo/price -> pagamento -> conferma.
   - Implementazione: [`PurchaseReservationFragment.kt`](FS_PARK_Android/app/src/main/java/it/fsitaliane/fspark/fragments/sosta/PurchaseReservationFragment.kt:1), [`PlatesReservationFragment.kt`](FS_PARK_Android/app/src/main/java/it/fsitaliane/fspark/fragments/sosta/PlatesReservationFragment.kt:1).

2. Selezione parcheggio, data/ora, targa
   - I dati del parcheggio (parkingId, nome, indirizzo) sono popolati nel singleton `reservationFlowData` o passati via args.
   - Selezione targa: l'utente può scegliere una targa dall'account (`singleton.userData.platesList`) o aggiungerne una nuova (flusso Add Plate). Vedere `PlatesReservationFragment` per la logica di popolamento.

3. Calcolo prezzo
   - L'app richiede al servizio (o usa mock) il prezzo per l'intervallo selezionato. Esempio di risposta mockata: [`MockParkingReservationWithPrice.json`](FS_PARK_Android/app/src/main/assets/mocks/MockParkingReservationWithPrice.json:1) che contiene fields: parkingAvailable, price, currency.
   - La UI mostra prezzo formattato e simbolo valuta, oppure nasconde l'area prezzo se il prezzo è 0 o non disponibile.

4. Conferma e pagamento
   - Pagamento gestito tramite webview/modal payment o integrazioni con provider (EasyPark, Telepass, ecc.) visibili come opzioni in UI.
   - In flussi di modifica (supplemento/riduzione) il comportamento varia: riduzione non richiede pagamento, supplemento richiede pagamento (vedi `PurchaseReservationFragment.setFlowType`).

Business rules
--------------
- Timer di prenotazione: quando viene mostrato il riepilogo con prezzo, un timer di scadenza (reservationExpiryTimestamp) viene avviato e mostrato in UI; alla scadenza la sessione è invalidata e l'utente deve ricominciare.
- Se `price <= 0` o flusso è reduce -> non mostrare componenti di pagamento e mostrare pulsante full-width di conferma.
- Se `parkingAvailable` è false il sistema mostra errore e offre alternative o retry.

UI Components
-------------
- `fragment_purchase_reservation.xml` (view binding: `FragmentPurchaseReservationBinding`) con sezione `viewReservation` che contiene priceContainer, countdownContainer e pulsanti di conferma.
- `fragment_plates_reservation.xml` con lista targhe, selettore e bottom sheet per aggiunta targa.

Stati e validazioni
--------------------
- Stati: Loading, Ready (with reservation data), Expired, Error (unavailable parking, payment error).
- Validazioni principali: esistenza targa selezionata per procedere, disponibilità parcheggio, validità intervallo temporale (check-out > check-in).

Error handling
--------------
- Prenotazione scaduta: mostra CustomDialog con possibilità di ricominciare.
- Payment pending/error: mostra dialog con opzione di ricominciare pagamento o contattare supporto.

Riferimenti tecnici
-------------------
- Purchase flow: [`PurchaseReservationFragment.kt`](FS_PARK_Android/app/src/main/java/it/fsitaliane/fspark/fragments/sosta/PurchaseReservationFragment.kt:1)
- Plate selection: [`PlatesReservationFragment.kt`](FS_PARK_Android/app/src/main/java/it/fsitaliane/fspark/fragments/sosta/PlatesReservationFragment.kt:1)
- Mock prezzo: [`MockParkingReservationWithPrice.json`](FS_PARK_Android/app/src/main/assets/mocks/MockParkingReservationWithPrice.json:1)

