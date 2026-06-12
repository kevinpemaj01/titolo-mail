# Titolo Mail — Add-in Outlook per Studio Pemaj

Genera con un click un titolo riassuntivo della mail aperta nel formato:

    AAAA_MM_GG - [Tipo] [a/da] [controparte esterna] per [motivo]

Esempi:
- `2026_06_12 - Risposta a Enzo per preventivo cabina MT Quattordio`
- `2026_06_10 - Richiesta da Enzo per documentazione antincendio Chieri`

Le persone "interne" (Studio Pemaj, Studio Gallo, Tecnomisure — lista modificabile nelle impostazioni del pannello) non vengono mai nominate: nel titolo compare solo la controparte esterna. Se la mail parte da un interno è "… a Enzo", se arriva da un esterno è "… da Enzo". Le risposte (R:/RE:) diventano sempre "Risposta", gli inoltri (I:/FW:) "Inoltro"; per le altre mail il tipo (Richiesta, Invio, Sollecito, Conferma…) lo sceglie l'AI, oppure senza chiave il titolo resta "Da/A Enzo per [oggetto]".

Nei thread di risposta viene analizzato **solo l'ultimo messaggio** (il testo viene tagliato al primo "Da:" / "Messaggio originale" citato).

Il pulsante **Salva .eml…** apre direttamente la finestra "Salva con nome" (Edge/Chrome) con il titolo già compilato: scegli la cartella OneDrive e fine, senza passaggi intermedi. Se il browser non supporta la finestra, parte un download classico: in quel caso attiva in Edge → Impostazioni → Download → "Chiedi dove salvare ogni file prima di scaricarlo" per avere comunque la scelta della cartella.

## Contenuto della cartella

| File | A cosa serve |
|---|---|
| `manifest.xml` | Il "documento d'identità" dell'add-in, da caricare in Outlook |
| `taskpane.html` | Il pannello vero e proprio (interfaccia + logica) |
| `icon-*.png` | Icone del pulsante |

## Installazione (una volta sola, ~15 minuti)

L'add-in deve essere ospitato su un sito HTTPS. Il modo gratuito più semplice è GitHub Pages:

### 1. Pubblica i file su GitHub Pages

1. Crea un account su github.com (se non lo hai già).
2. Crea un nuovo repository **pubblico** chiamato `titolo-mail`.
3. Carica dentro tutti i file di questa cartella (Add file → Upload files).
4. Vai in Settings → Pages → Source: **Deploy from a branch**, branch `main`, cartella `/ (root)` → Save.
5. Dopo qualche minuto il sito sarà attivo su `https://TUO-UTENTE.github.io/titolo-mail/`.

### 2. Sistema il manifest

Apri `manifest.xml` e sostituisci **tutte** le occorrenze di `TUO-UTENTE` con il tuo nome utente GitHub (Trova e sostituisci, sono 8 punti). Ricarica il file aggiornato sul repository.

### 3. Carica l'add-in in Outlook

**Outlook sul web / nuovo Outlook per Windows:**
1. Apri una mail qualsiasi → menu `…` (App) → **Aggiungi app** → cerca "I miei componenti aggiuntivi" / "Add-in personalizzati".
2. Scegli **Aggiungi add-in personalizzato → Da URL** e incolla:
   `https://TUO-UTENTE.github.io/titolo-mail/manifest.xml`
   (in alternativa: "Da file" caricando il manifest.xml dal PC).
3. Conferma. Da quel momento aprendo una mail troverai il pulsante **Titolo Mail**.

L'add-in installato sul web compare automaticamente anche nel nuovo Outlook desktop e sull'app, perché è legato all'account M365.

## Uso quotidiano

1. Apri la mail → pulsante **Titolo Mail** → **Genera titolo**.
2. Controlla/ritocca il titolo proposto nel riquadro.
3. **Salva .eml…** → si apre la finestra di scelta cartella col nome già compilato → salvi direttamente dove vuoi.
   In alternativa **Copia titolo** e usalo in un "Salva con nome" manuale.

## Impostazioni nel pannello

- **Nomi/domini interni**: parole che identificano i "vostri" (default: pemaj, studio gallo, studiogallo, tecnomisure). Chi le contiene nel nome o nell'indirizzo non viene nominato nel titolo. Puoi aggiungere collaboratori o domini, separati da virgola.
- **Chiave API Anthropic** (opzionale): abilita il riassunto AI di tipo e motivo.

## Riassunto AI del "perché" (opzionale)

Senza configurazione, il "motivo" è l'oggetto della mail ripulito (senza RE:, I:, FW:…). Se vuoi un riassunto vero del contenuto:

1. Crea una chiave API su https://console.anthropic.com (costo: frazioni di centesimo per mail, modello Haiku).
2. Nel pannello apri "Impostazioni riassunto AI" → incolla la chiave → **Salva chiave**.

La chiave resta memorizzata solo nel browser/client in cui la inserisci e viene usata esclusivamente per la chiamata diretta all'API Anthropic.

## Limiti noti

- Il download `.eml` funziona con account Microsoft 365 / Exchange Online (usa il token REST dell'add-in). Se su un client non funzionasse, c'è sempre "Copia titolo" come ripiego.
- Le firme molto lunghe possono entrare nel testo analizzato dall'AI: il riassunto resta comunque limitato a ~8 parole.
- Outlook "classico" desktop mostra l'add-in solo se l'account è M365/Exchange (non POP/IMAP).
