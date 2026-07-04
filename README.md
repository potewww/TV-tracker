# 📺 TV Tracker

Sito statico personale per tracciare film e serie viste, aggiornabile modificando
un semplice file CSV direttamente dall'interfaccia web di GitHub — nessuna build,
nessuna Action, nessun terminale necessario dopo il setup iniziale.

## Setup (una tantum)

1. Crea un nuovo repository su GitHub (può essere pubblico o privato, ma per
   GitHub Pages gratuito deve essere **pubblico**, a meno che tu non abbia
   GitHub Pro/Team).
   - Nome suggerito: `tv-tracker`
2. Carica in questo repo i 3 file di questo progetto: `index.html`, `data.csv`, `README.md`.
   - Puoi farlo trascinandoli nella pagina "Add file → Upload files" su GitHub.
3. Vai su **Settings → Pages** (nel menu laterale del repo).
4. In "Build and deployment", scegli **Source: Deploy from a branch**.
5. Seleziona il branch `main` (o `master`) e la cartella `/ (root)`, poi **Save**.
6. Dopo 1-2 minuti il sito sarà live all'indirizzo:
   `https://<tuo-username>.github.io/<nome-repo>/`

## Come aggiungere un film/serie visto

1. Vai nel repo su GitHub, apri `data.csv`.
2. Clicca sull'icona della matita (✏️ Edit this file).
3. Aggiungi una nuova riga in fondo, seguendo il formato:

   ```
   Titolo,Tipo,YYYY-MM-DD,voto,nota
   ```

   Esempio:
   ```
   Dune Part Two,Film,2026-07-01,9,Visto al cinema in IMAX
   ```

   - `Tipo` deve essere esattamente `Film` o `Serie` (per far funzionare il filtro).
   - `voto` è un numero da 0 a 10 (anche con decimali, es. 8.5).
   - `nota` è opzionale, ma se contiene una virgola vanno messe le virgolette:
     `"Bello, ma lungo"`.

4. In fondo alla pagina clicca **Commit changes...** → **Commit directly to the main branch**.
5. Ricarica il sito (`https://<tuo-username>.github.io/<nome-repo>/`): la nuova
   riga apparirà automaticamente. GitHub Pages di solito si aggiorna in 30-60 secondi.

## Come funziona

`index.html` non fa nessuna build: al caricamento della pagina, il JavaScript
scarica `data.csv` direttamente dal repository (tramite la libreria PapaParse)
e lo trasforma in una lista con ricerca, filtro per tipo e ordinamento.
Ogni volta che modifichi `data.csv` su GitHub, il sito pubblicato mostra
subito i dati aggiornati, perché li legge "al volo" a ogni visita.

## Importare i dati vecchi da TV Time

Se hai già l'export CSV di TV Time, apri quel file e il nuovo `data.csv`
affiancati, e copia i dati riga per riga adattando le colonne al formato
sopra (`title,type,date_watched,rating,note`). Se mi mandi un estratto
del CSV originale di TV Time posso aiutarti a scrivere uno script che
faccia la conversione in automatico, invece di farlo a mano.

## Idee per estensioni futuro

- Poster automatici recuperando i dati da TMDB (richiede una API key e
  qualche riga di JS in più).
- Statistiche più avanzate (grafico voti nel tempo, generi preferiti).
- Versione mobile-friendly con pulsante "aggiungi" che apre direttamente
  l'editor GitHub in una nuova scheda.

Chiedi pure se vuoi una di queste funzionalità aggiunta.
