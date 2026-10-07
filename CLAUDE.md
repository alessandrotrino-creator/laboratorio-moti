# CLAUDE.md — Laboratorio dei Moti

Note per riprendere il lavoro. L'insegnante scrive in italiano: rispondi in italiano, in modo semplice.

## Chi e per cosa
- Docente di matematica e scienze, scuola secondaria di primo grado. Uso: LIM, lavoro autonomo, compiti.
- Progetto "sorella" del Laboratorio dei Solidi e del Laboratorio di Fisica 2D: stesso stile (token colore,
  pulsanti grandi, tema chiaro/scuro, «Alta leggibilità», esercizi in serie da 10 con autocorrezione).
- Sul PC del docente non c'è git: i file si caricano dal sito di GitHub (Add file → Upload files).

## Decisioni prese
- **Un solo file** `index.html`, nessuna libreria, nessun build; deve funzionare anche aprendo il file con
  doppio clic. I font Google sono facoltativi.
- Notazione da libro italiano: divisione «:», moltiplicazione «·», virgola decimale, **π = 3,14**, g = 9,8 m/s².
- Nei calcoli i dati vengono prima trasformati in m, s, m/s (passaggio «Trasformo in …» mostrato), poi il
  risultato è riportato anche nell'unità scelta. Eccezione: nei «Due corpi» con km e km/h si lavora in
  km, km/h e h, come sul quaderno.
- Emoji: niente emoji recenti (Windows 10). I corpi sono disegnati sul canvas (`DRAW`).
- **Usato soprattutto sulla LIM**: interruttore «Vista LIM» (`S.lim`, classe `body.lim`, fattore `UI()` per
  le scritte del canvas); si accende da solo alla prima apertura su uno schermo grande e tattile.
  «Schermo intero» porta a tutto schermo animazione, comandi e grafici insieme (`.stage`).

- Numeri arrotondati a **due decimali** (`decs`); solo i numeri che diventerebbero 0 tengono due cifre
  significative (es. 0,0023 s).
- Le caselle delle letture sopra la scena **non devono muoversi** (richiesta del docente: «sfarfallavano»):
  `readout` crea le caselle una volta e aggiorna solo i valori; `prescanReadout` prova il moto in anticipo
  (istanti regolari e irregolari) e fissa `min-width` sulla casella più larga. Va richiamata quando cambiano
  dati, tema, caratteri o vista LIM.
- Menu dei moti (8 voci, `PICK_MOTI`): alcune voci sono gruppi di "situazioni" (`GROUPS`): «Caduta e lancio
  in alto» (`caduta`, `lancioSu`), «Moto parabolico» (`oriz`, `obli`), «Moti composti» (`fiume`, `tapis`).
  Convenzione: nei grafici la velocità verso l'alto è positiva.

## Sezioni
1. **Laboratorio**: tipo di moto → corpo → dati. L'utente scrive solo i dati che conosce; il risolutore
   ricava gli altri e mostra i passaggi. Scena animata, grafici s–t, v–t (e a–t), «foto a intervalli»,
   vettori, ombra (proiezione: circolare → armonico; lanci → moti sugli assi).
2. **Esercizi**: 3 livelli × argomenti (conversioni, MRU, MRUA, circolare, armonico, incontri, composti,
   grafici). Domande numeriche, scelta multipla, lettura di grafici. «Vedi il moto nel laboratorio».
3. **Formule**: scheda riassuntiva stampabile, generata dagli stessi dati del risolutore.

## Mappa del codice (tutto in un'unica IIFE)
| Parte | Contenuto |
|---|---|
| `fmt`, `parseNum`, `N` | numeri all'italiana (spazio fine per le migliaia) |
| `KIND`, `CONV`, `VARS` | grandezze, unità, simboli |
| `MOTI` | ogni moto: `vars`, `def` (dati sottintesi), `nomi`, `rel` = relazioni `R(nome, {eq, extra, only}, fm(...))`; ogni `fm(incognita, dati, formula, sostituzione, calcolo, condizione)` è una formula diretta o inversa |
| `solve` | applica le formule finché può; controlla la coerenza con `eq`; `SUBSIS` sceglie le relazioni `only` (molla/pendolo, tapis stesso/opposto) |
| `KIN` | posizione, velocità, accelerazione nel tempo per la simulazione |
| `dPos`, `dVel`, `meet`, `computeDue` | due corpi: istante d'incontro esatto (a tratti, anche con partenze ritardate e accelerazioni) |
| `buildSim`, `SIM`, `loop` | animazione; `SIM.base` = ritmo automatico, `S.mult` = ritmo scelto |
| `drawLinear`, `drawCircular`, `drawHarmonic`, `drawDue`, `drawFiume`, `drawTapis`, `drawLancio` | scene |
| `plot`, `autoPlot`, `drawGraphs` | grafici (più curve, punto «adesso», punto d'incontro) |
| `renderPanel`, `compute` | pannello e calcoli passo passo; «Nascondi risultati» con `S.revealed` |
| `GENS`, `QUIZ` | esercizi: `{lvl, cat, make()}` → `text`, `hint`, `asks` (`A(...)`), `sol` (`L(...)`), `graph`, `lab`, oppure `choice`. Nelle domande di `QUIZ` la prima risposta è quella giusta (vengono mescolate) |
| `renderFo` | scheda formule |

Per aggiungere un moto: una voce in `MOTI` (+ `KIN`, una funzione di disegno, i grafici in `drawGraphs`,
`ESEMPI`). Per aggiungere un esercizio: un oggetto in `GENS`.

## Come provare
Server locale: `powershell -ExecutionPolicy Bypass -File C:\Users\Ale\Desktop\enigmistica\strumenti\server.ps1
-Root C:\Users\Ale\Desktop\laboratorio-moti -Port 8767` (la porta 8766 è usata da un altro progetto).
Hook per i test: `window.MOTI_LAB`
(`S`, `SIM`, `solve`, `GENS`, `compute`, `openInLab`, `getRES`…).
Controlli fatti (07/10/2026): tutti gli esempi di ogni moto; 16 800 esercizi generati senza NaN e con ogni
risposta presente nella soluzione; 181 aperture «Vedi nel laboratorio» coerenti; serie completa risolta
dall'interfaccia; viste PC e smartphone, tema scuro.

## Proposte aperte
- Caduta libera in verticale come moto a sé (oggi: MRUA con il razzo, oppure il lancio orizzontale).
- Esercizi stampabili / verifica cartacea con soluzioni a parte.
