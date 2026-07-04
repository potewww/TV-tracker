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
scarica `data.csv` (e `seasons.csv`, se presente) direttamente dal repository
(tramite la libreria PapaParse) e li trasforma in una lista con ricerca,
filtro per tipo e ordinamento. Ogni volta che modifichi i CSV su GitHub, il
sito pubblicato mostra subito i dati aggiornati, perché li legge "al volo"
a ogni visita.

## Aggiungere il dettaglio delle stagioni (solo per le Serie)

Apri (o crea) il file `seasons.csv` nel repo con queste colonne:

```
title,season,total_episodes,watched_episodes
```

Una riga per ogni stagione. Esempio:

```
Breaking Bad,1,7,7
Breaking Bad,2,13,13
Breaking Bad,3,13,13
Breaking Bad,4,13,13
Breaking Bad,5,16,10
```

Importante: il `title` deve essere scritto **esattamente come in `data.csv`**
(le maiuscole/minuscole non contano, ma il testo deve corrispondere).
Se una serie non compare in `seasons.csv`, viene mostrata come prima senza
dettaglio stagioni — nessun obbligo di compilarlo per tutto.

Nel sito, cliccando sulla riga della serie si apre l'elenco delle stagioni
con una barra di progresso (episodi visti / episodi totali) per ciascuna.

## Link a Wikipedia

Ogni titolo ha già, in automatico, un link "Wikipedia ↗" che apre
`https://it.wikipedia.org/wiki/<Titolo>`. Non serve inserire nulla a mano:
se la pagina con quel nome esatto non esiste, Wikipedia stessa mostra i
risultati di ricerca più vicini. Se un titolo ha un nome molto ambiguo
(es. un film con un titolo molto generico) il link potrebbe non centrare
la pagina giusta al primo colpo: in quel caso basta cercare manualmente
una volta su Wikipedia.

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
