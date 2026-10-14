# Revisione v2 della tesi

Questo branch (`claude-revisionato`) parte da una copia esatta di `claude/awesome-ride-pfilsf` (commit `3f48a00`) e applica il feedback ricevuto sulla v1.

## File

| File | Contenuto |
|---|---|
| `Capobianco_Thesis_v2.docx` | Versione revisionata, pulita. |
| `Capobianco_Thesis_v2_revisioni.docx` | Stessa versione, con tutte le modifiche rispetto alla v1 mostrate come revisioni di Word (autore "Revisione v2"). Da Word: *Revisione → Accetta / Rifiuta*. Accettando tutto si ottiene esattamente la v2. |
| `Capobianco_Thesis_v1.docx` | Invariata, per confronto. |

Il sommario si aggiorna quando apri il file in Word (alla domanda sull'aggiornamento dei campi rispondi Sì). In LibreOffice e nelle anteprime appare vuoto, come già nella v1.

## Punti del feedback

| # | Punto | Applicato | Cosa è stato fatto |
|---|---|---|---|
| 1 | Domanda di ricerca e quattro dimensioni ripetute | Sì | La §1.4 diventa *Research aim*: un paragrafo breve che rimanda alla §4.4. La domanda completa e la definizione delle quattro dimensioni restano solo in §4.4, con una frase su cosa chiede ciascuna dimensione. Le altre parti rimandano a "the four dimensions of Section 4.4". Uniformato *self-service exploration* (la §4.4 diceva *self-service access*). |
| 2 | Metodo separato dall'implementazione | Sì | La §5.1 diventa *Research design and overall approach*, con tre sottosezioni: 5.1.1 *Research design and evidence* (unità di analisi, cinque fonti di evidenza, procedura di confronto, limiti dell'osservazione partecipante), 5.1.2 *Architecture of the solution*, 5.1.3 *Contribution of the author*. |
| 3 | Ripetizioni tra capitoli 4, 6 e 8 | Sì | Il cap. 6 non ripete più la descrizione del "prima" e del "dopo": ogni sezione parte dal meccanismo e lo interpreta. Il cap. 8 è più breve. |
| 4 | Conclusioni troppo nette | Sì | Tolta la frase "The improvement is therefore real along all four dimensions". Ora la §8.2 dice: *structural improvements along all four dimensions, although the magnitude of these improvements and their realised organisational effect were not measured*. |
| 5 | Abstract | Sì | Riscritto in tre paragrafi: contesto e domanda; metodo ed evidenze; risultati, limiti e contributo. Il doppio spazio in "during my  internship" non c'è nella v1 (il revisore aveva probabilmente un'altra versione); ho comunque controllato i doppi spazi in tutto il testo e non ce ne sono. |
| 6 | Tabelle di sintesi | Sì | Nuova Tabella 2 (fasi della pipeline: responsabilità, caricamento, cosa succede in caso di errore) in §5.2.7; nuova Tabella 3 (alternative di modellazione: esito, vantaggio, costo) in §5.3.3; nuova Tabella 5 (problema di qualità dei dati, protezione, controllo, rischio residuo) in §5.6.4. Il catalogo dei KPI ora è la Tabella 4. |
| 7 | Figure mancanti | Non necessario | Le quattro figure sono incorporate nel Word e si vedono: l'ho verificato convertendo il documento in PDF. Probabilmente il revisore ha letto solo il testo estratto, senza immagini. Nessuna modifica. |
| 8 | Terminologia | Sì | *program* → *programme*; *on-premise* → *on-premises*; *decision-support* → *decision support*; *didactic offering / services / activities / organisation* → *academic offering / teaching services office / teaching activities / teaching organisation*; *up-to-date* → *up to date*. "Chapter N" per i capitoli e "Section N.N" per le sezioni. I titoli "4. Problem Definition" e "5. Research methods…" sono stati uniformati agli altri capitoli. |
| 9 | Frasi troppo lunghe | Sì | Frasi divise e un concetto per paragrafo in §5.2.4, §5.3.3, §5.4.1, §5.6.2 (e §5.5.2). |
| 10 | Discussione più analitica | Sì | §6.1: quali richieste spariscono e quali richiedono ancora sviluppo. §6.2: il rischio che la governance centralizzata diventi un nuovo collo di bottiglia, e chi possiede le definizioni dopo il passaggio di consegne. §6.3: l'effetto della mancata sincronizzazione dei filtri sulla fiducia degli utenti. §6.4: una dipendenza strutturale è meno tollerante ai guasti. Nuova §6.5 sulla trasferibilità. *Limitations* diventa §6.6 e *Managerial implications* §6.7. |
| 11 | Autorialità ripetuta | Sì | Nuova Tabella 1 in §5.1.3 (chi ha fatto cosa, per componente). Accorciate le ripetizioni in §1.2, §1.5, §2.3, §5.2.7, §5.3, §5.4, §5.6. **Corretta un'incoerenza vera**: nella v1 la §5.1 diceva che il tuo lavoro sul gold layer era "limited to consuming the gold layer", mentre abstract, §5.3, §5.6 e §7.1 dicono che le tabelle gold le hai costruite tu. |
| 12 | Bibliografia e fonti | Sì | Aggiunte cinque fonti di documentazione, verificate il 14 ottobre 2026: Databricks (architettura medallion), Snowflake (Dynamic Tables, Time Travel), Microsoft (relazioni in Power BI, star schema). Sono citate in §5.2, §5.2.5, §5.3.2 e §5.3.3. Formato uniforme: libri e riviste in corsivo, titoli di articoli e pagine web tra virgolette. **Corretta una citazione imprecisa**: in §5.6.2 la v1 attribuiva a Wang and Strong (1996) anche *uniqueness* e *validity*, che non fanno parte delle loro dimensioni. |

## Altre correzioni trovate durante la revisione

- §5.6.3 chiamava la *Fascia Studente CdS* una misura DAX, mentre §5.4.3 la descrive (correttamente) come colonna calcolata nel gold layer. Inoltre la soglia per "N/D" era descritta come "nessuna lezione registrata" invece di "due lezioni o meno". Allineate.
- §5.5.2 parlava di fascia "gialla o rossa", ma le fasce sono quattro (inclusa la nera): ora dice "the higher risk bands".
- Impaginazione: le righe delle tabelle non si spezzano più tra due pagine, e le tabelle brevi restano unite alla loro didascalia.

## Da confermare

Queste parti le ho scritte deducendole dal testo, senza poterle verificare:

1. **Tabella 1, riga "Reports and their distribution as a Power BI application"**: attribuita al *delivery team*. Se le pagine di `Report_Didattica` le hai costruite tu, in tutto o in parte, va corretta.
2. **§5.1.1, fonti di evidenza**: "kick-off material" e "table descriptions" vengono dalle note in `progress.md`. La frase "the project's account of the existing situation" presuppone che la descrizione del processo precedente venga dalla documentazione di progetto e dagli scambi con il personale dell'università: confermi?
3. **§6.2**: dico che la tesi non osserva chi gestisce le definizioni dei KPI dopo il passaggio di consegne. Se lo sai (KPMG, l'IT dell'università o entrambi), si può scrivere esplicitamente.

## Lunghezza

Da 49 a 54 pagine con la stessa impaginazione (da circa 15.200 a 16.900 parole). Metodo, tabelle e discussione aggiungono testo, e le ripetizioni tolte lo compensano solo in parte.

## Nota sui file markdown

I file `section-*.md` sono le bozze della v1 e non sono stati aggiornati. Da questa revisione in poi il riferimento è `Capobianco_Thesis_v2.docx`.
