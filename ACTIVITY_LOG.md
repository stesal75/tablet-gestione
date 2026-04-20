## Sessione 2026-04-20 — pomeriggio

### Attività svolte
- Inizializzato repo Git locale e creato repo pubblico su GitHub (`stesal75/tablet-gestione`)
- Installato GitHub CLI (`gh`) via winget
- Configurato deploy automatico su Vercel tramite GitHub
- Aggiunto pulsante "Seleziona cliente" nel tastierino
- Implementata modale clienti con lista, ricerca, aggiunta, modifica, eliminazione
- Campi cliente: Nome, Cognome, Telefono, Email, Automobile, Targa
- Persistenza clienti in localStorage
- Aggiunto pulsante 📷 accanto al campo Targa per OCR
- OCR: tentato con Tesseract.js (troppo lento), poi API Ninjas (parsing errato), poi Google Cloud Vision
- Risolto problema binarizzazione immagine che impediva il riconoscimento
- Aggiornata API key Google Vision (billing abilitato)
- Regex targa impostata su formato fisso `AA999AA`
- Validazione form: basta un campo tra Nome/Cognome/Telefono/Targa

### Modifiche ai file
- `index.html` — aggiunta completa gestione clienti + OCR targa

### Problemi risolti
- Tesseract.js bloccava per minuti → rimosso, sostituito con API esterna
- API Ninjas restituiva `[object Object]` → parsing errato dell'array
- Google Vision 403 → mancava billing sul progetto GCP → nuova chiave con billing
- Preprocessing bianco/nero impediva lettura a Google Vision → rimosso, si invia JPEG a colori
- Doppia funzione `preprocessPlate` duplicata → cleanup

### Problemi aperti / TODO
- Valutare se aggiungere un ritaglio manuale/regolabile per migliorare OCR
- Le API key sono hardcoded in index.html (visibili nel sorgente)

---
