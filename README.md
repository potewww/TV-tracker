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

## Struttura dei dati

Il sito usa due file, ognuno con un solo compito:

- **`data.csv`** → SOLO film. Colonne: `title,type,date_watched,rating,note,status`
- **`seasons.csv`** → SOLO serie, una riga per ogni stagione. Colonne: `title,season,total_episodes,watched_episodes,date_watched`

## Segnare un film come "da vedere" (non ancora visto)

Nella colonna `status` di `data.csv`, scrivi `NV` (Non Visto). Lascia le
altre colonne vuote (non ha senso avere una data di visione o un voto per
qualcosa che non hai ancora visto):

```
Dune: Part Three,Film,,,,NV
```

Se `status` è vuoto, il film viene considerato visto normalmente.

## Segnare una serie come "da vedere"

Basta NON metterla in `seasons.csv`, oppure metterla con `watched_episodi=0`
in tutte le sue righe:

```
The Wire,1,,0,
```

Il sito riconosce automaticamente che una serie è "da vedere" quando la
somma degli episodi visti in tutte le sue stagioni è zero. Appena aggiungi
almeno un episodio visto (`watched_episodes` > 0 in una qualsiasi riga),
la serie si sposta da sola nella sezione principale.

## Le due sezioni del sito

- **Sezione principale** (in alto): tutto ciò che hai già visto, con
  ricerca, filtro per tipo, ordinamento e dettaglio stagioni per le serie.
- **📌 Da vedere** (in basso): film con `status=NV` e serie con 0 episodi
  visti, in una lista semplice con ricerca e filtro propri.

## Aggiungere un film visto

Apri `data.csv` su GitHub e aggiungi una riga:

```
Titolo,Film,YYYY-MM-DD,voto,nota,
```

Esempio:
```
Dune Part Two,Film,2026-07-01,9,Visto al cinema in IMAX,
```

## Aggiungere una serie vista (o una nuova stagione)

Apri `seasons.csv` e aggiungi una riga per ogni stagione:

```
title,season,total_episodes,watched_episodes,date_watched
```

Esempio (aggiungere una nuova serie "The Wire" con 2 stagioni viste):
```
The Wire,1,13,13,2026-05-01
The Wire,2,12,8,2026-06-15
```

- Il `title` deve essere scritto uguale in tutte le righe della stessa serie.
- `total_episodes` puoi lasciarlo vuoto se non lo conosci: il sito mostrerà
  comunque gli episodi visti, solo senza barra di progresso.
- `date_watched` è opzionale per singola riga: il sito usa la data più
  recente tra tutte le stagioni della serie per l'ordinamento cronologico.

## Come funziona

`index.html` non fa nessuna build: al caricamento della pagina, il JavaScript
scarica `data.csv` e `seasons.csv` direttamente dal repository (tramite la
libreria PapaParse). I film vengono presi tali e quali da `data.csv`. Le
serie vengono ricostruite raggruppando le righe di `seasons.csv` per titolo:
il sito calcola da solo episodi visti/totali e la data più recente. In base
allo `status` e alla somma degli episodi visti, ogni titolo finisce nella
sezione "visti" o "da vedere". Ogni volta che modifichi i CSV su GitHub, il
sito pubblicato mostra subito i dati aggiornati.

## Aggiungere il dettaglio delle stagioni

Cliccando sulla riga di una serie vista (quella con la freccia ▶) si apre
l'elenco delle stagioni con una barra di progresso (episodi visti / episodi
totali) per ciascuna, calcolata automaticamente da `seasons.csv`.

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
