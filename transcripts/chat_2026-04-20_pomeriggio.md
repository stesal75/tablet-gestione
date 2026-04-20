# Trascrizione Sessione — 2026-04-20

## Sommario
Sessione dedicata alla pubblicazione dell'app su GitHub/Vercel e all'implementazione della gestione clienti con OCR targa. L'app è ora live su Vercel con deploy automatico e OCR funzionante tramite Google Cloud Vision API.

## Richieste Principali dell'Utente
1. Pubblicare l'app web via GitHub su Vercel
2. Aggiungere pulsante selezione cliente nel tastierino (campi: Nome, Cognome, Telefono, Email, Automobile, Targa)
3. Permettere salvataggio cliente con solo telefono e targa
4. Aggiungere pulsante 📷 per fotografare la targa e tradurla in testo (OCR)
5. Impostare regex targa formato italiano fisso: 2 lettere + 3 numeri + 2 lettere
6. Debug OCR non funzionante
7. Passare da API Ninjas a Google Cloud Vision per l'OCR

## Soluzioni Adottate

### Deploy
- Installato GitHub CLI via winget
- Autenticazione tramite token personale `gh auth token`
- Creato repo pubblico `stesal75/tablet-gestione` con `gh repo create --public --source=. --push`
- Vercel collegato al repo GitHub per deploy automatico

### Gestione Clienti
- Modale con lista ricercabile, form aggiunta/modifica, eliminazione
- Persistenza in localStorage con chiave `tg_customers`
- Validazione minima: almeno uno tra Nome, Cognome, Telefono, Targa
- Anagrafica in cima all'app si aggiorna con i dati del cliente selezionato

### OCR Targa
- Tentativo 1: Tesseract.js — bloccava per 2+ minuti su mobile
- Tentativo 2: API Ninjas — risposta parsata male (`[object Object]`)
- Soluzione finale: Google Cloud Vision `TEXT_DETECTION`
- Preprocessing: solo scala a max 1600px JPEG, colori originali (la binarizzazione peggiorava)
- Regex: `/[A-Z]{2}[0-9]{3}[A-Z]{2}/`

## Decisioni Tecniche Rilevanti
- **Nessuna binarizzazione immagine**: Google Vision riconosce meglio le immagini a colori
- **JPEG invece di PNG**: file più leggero da inviare all'API
- **localStorage**: sufficiente per un'app tablet locale, no backend necessario
- **Single file HTML**: semplicità di deploy e manutenzione

## Errori Incontrati e Fix
- **Errore:** Tesseract.js bloccato su "lettura targa" per minuti
  **Fix:** Sostituito con API esterna

- **Errore:** API Ninjas restituisce `[object Object]`
  **Fix:** Parsing corretto array/oggetto + log debug

- **Errore:** Google Vision HTTP 403 "billing required"
  **Fix:** Abilitato billing su GCP, nuova API key

- **Errore:** OCR non trova testo ("testo: " vuoto)
  **Fix:** Rimossa binarizzazione bianco/nero che distruggeva l'immagine

- **Errore:** Doppia funzione `preprocessPlate` nel codice
  **Fix:** Cleanup, rimasta solo la versione corretta
