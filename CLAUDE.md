# Tablet Gestione

## Stack
- HTML/CSS/JS puro (single file `index.html`)
- Nessun framework, nessun build step
- Deploy: Vercel (collegato a GitHub repo `stesal75/tablet-gestione`)
- Storage: localStorage per i clienti

## Struttura File Principali
- `index.html` — l'intera applicazione (CSS + HTML + JS in un unico file)

## Setup
Nessun setup richiesto. Aprire `index.html` nel browser o visitare l'URL Vercel.

Per aggiornare il deploy:
```bash
export PATH="$PATH:/c/Program Files/GitHub CLI"
export GH_TOKEN="<token>"
cd C:/Users/info/progetti-claude/tablet-gestione
git add index.html && git commit -m "..." && git push
```
Vercel fa il deploy automatico ad ogni push su `master`.

## Stato
**Funzionante e pubblicato su Vercel.**

Funzionalità implementate:
- Pannello superiore: anagrafica cliente + lista servizi con categorie colorate
- Pannello inferiore: tastierino calcolatore + pannello totali per categoria
- Gestione clienti: modale con lista, ricerca, aggiunta, modifica, eliminazione
- Campi cliente: Nome, Cognome, Telefono, Email, Automobile, Targa
- OCR targa: foto → Google Cloud Vision API → regex `AA999AA` → campo targa
- Clienti salvati in localStorage (persistono tra sessioni)

## Note Importanti

### GitHub & Vercel
- Repo: `https://github.com/stesal75/tablet-gestione`
- Account GitHub: `stesal75` (stefano.salvoni@gmail.com)
- Deploy automatico su Vercel ad ogni push

### Google Cloud Vision API
- Chiave attiva: `AIzaSyCtBZjpbJCi-FkLcMGxNSqGqsHp52N4_dk`
- Progetto GCP: #255073236459
- Richiede billing abilitato (1000 req/mese gratis)
- Endpoint: `POST https://vision.googleapis.com/v1/images:annotate`
- L'immagine viene inviata in base64 JPEG (max 1600px, colori originali — NO binarizzazione)

### OCR Targa
- Regex: `/[A-Z]{2}[0-9]{3}[A-Z]{2}/` (formato italiano AA999AA)
- Il preprocessing converte in JPEG 92% qualità senza filtri bianco/nero
- La binarizzazione era stata rimossa perché peggiorava il riconoscimento

### Validazione cliente
- Basta compilare almeno uno tra: Nome, Cognome, Telefono, Targa
- Se tutto vuoto si evidenziano in rosso Telefono e Targa

*Ultimo aggiornamento: 2026-04-20*
