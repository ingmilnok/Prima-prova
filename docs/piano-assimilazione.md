# Piano di assimilazione dei testi ancora da leggere (creato il 15/10/2026)

Regola di costo: **una chat (sessione) per blocco**, **Sonnet** per lettura e sintesi, **Opus** una sola volta per la revisione finale. Le sessioni girano **una alla volta** sul ramo `claude/sintesi-leggi-regionali-v1-6` (stesso file `docs/sintesi-normativa-ottaviano.md`, niente conflitti). Ogni sessione chiusa viene archiviata dall'orchestratore.

## Protocollo di ogni sessione
1. Leggere `CLAUDE.md` (sezioni 2, 5, 6), questo file e, della sintesi, solo l'intestazione, le sezioni 8, 9, 10 e le sezioni che il blocco aggiorna (usare `grep -n '^## '` per trovarle).
2. Leggere i file dal Drive con il connettore Google Drive (`read_file_content`; se il risultato è troppo grande viene salvato su file: estrarre il testo con `python3 -I`). Se il testo è illeggibile o è una scansione: OCR (`pdftoppm` + `tesseract`, vedi CLAUDE.md sez. 6) e dirlo nella sintesi. Se il connettore Drive non è disponibile: **fermarsi** e dirlo, senza inventare nulla.
3. Non inventare norme; citare solo ciò che si legge; segnalare i conflitti senza risolverli (CLAUDE.md sez. 2). Decisioni già prese dall'utente: CLAUDE.md sez. 4 e sintesi sez. 8.
4. Aggiungere le sezioni numerate in fondo alla sintesi (dopo l'ultima), aggiornare sez. 8 (conflitti), 9 (testi mancanti), 10 (campi scheda), l'intestazione (versione +0.1) e CLAUDE.md sez. 3 e 5.
5. Aggiornare **questo file**: stato del blocco = fatto, e le **domande per l'utente** in fondo.
6. `git pull --rebase origin claude/sintesi-leggi-regionali-v1-6`, commit (con le righe di attribuzione richieste), `git push origin HEAD:claude/sintesi-leggi-regionali-v1-6`. **Nessuna pull request.**
7. Rispondere in italiano: cosa è cambiato, cosa manca, conflitti e domande. Non attendere risposte dall'utente: le domande vanno registrate qui.

## Blocchi

| # | Modello | Contenuto (ID file nella cartella Drive `norme`) | Sezioni sintesi | Stato |
|---|---|---|---|---|
| 1 | Sonnet | PSAI Allegato B (`1l1R2FriNXLpngVNTUjQhLOs3bb8kJQId`, `ALL_ B_COMP_GEOL_ 2015.pdf`) e Allegato D (`1beAMdeR5zTfvTLBohINQ3QhEl5iumEwK`, `ALL_D_TOTALE_2015.pdf`, 7,8 MB) | 27 | **fatto** (15/10/2026, v1.8) |
| 2 | Sonnet | Allegato 1 NTA, indici urbanistici (`1E_IMCn93RqpKdExSkUUd69ISVnjm1vTw`) e delibera n. 15 del 15/12/2025 su oneri e costo di costruzione (`1022gFmiMhprY0WC4Yy6swcdgwbkFOyWE`) | 28, 29 (+ sez. 3 e 7 se cambiano) | **fatto** (15/10/2026, v1.9) |
| 3 | Sonnet | Allegato 1 L. 326/2003 (`1FOGVegFuV6EttElXP4-xTSTuVmcd3Pz9`) e DPCM 12/12/2005 relazione paesaggistica (`1woRfD3L6Wv_3P1EIeq7ROcOXjdvk7BWf`) | 30 (+ aggiornare sez. 12 e 21), 31 (+ sez. 13) | **fatto** (15/10/2026, v2.0) |
| 4 | Sonnet | Allegato D.Lgs 222/2016 con la Tabella A (`1XP_dzuHaGwot1FihSSDpU2KE9gFf8L-p`, 19 MB, probabile scansione: OCR) e glossario unico DM 2/3/2018 (`1beL2Vu5QRVkTDxIZBl3Z5ZTykS5j_HB0`) | 32, 33 (+ sez. 15) | da fare |
| 5 | Sonnet | Piano di protezione civile: cartella `piano emergenza` (`1fRBr6yUj9WxLxe9H46axVrKnoo_iG3Z8`; decreto COC 17/2024, elaborati A e B) | 34 (+ sez. 5 bis e 17 se serve) | da fare |
| 6 | Opus | Revisione finale critica della sintesi, **una volta sola**: coerenza interna, riferimenti incrociati, conflitti, numeri di articolo, sezione 9 ripulita, passi per le istruzioni del Project. Scrivere le correzioni piccole nel file; le questioni grandi nelle domande per l'utente | tutte | da fare |

Dopo il blocco 6 restano i punti 3-6 di CLAUDE.md sez. 5 (scheda anonima, istruzioni del Project, modello PDF del parere, flusso Drive): non fanno parte dell'assimilazione.

## Domande per l'utente (le sessioni le aggiungono qui)
- **Blocco 1 – Allegato B PSAI**: per i progetti in area a rischio frana (R1-R4) con nuova edificazione, ampliamento con aumento di carico o interrati, il parere deve **prescrivere** la relazione di compatibilità geologica con relazione geotecnica (l'Allegato B §1 permette al Comune di richiederla) oppure limitarsi a elencarla tra i dati mancanti? Proposta: elencarla tra i dati mancanti e segnalare la facoltà del Comune.
- **Blocco 1 – Allegato D PSAI**: confermi che l'Allegato D (metodologie di studio per riperimetrazioni) non serve ai pareri e va richiamato solo se il progetto allega uno studio di riperimetrazione? Nella sintesi D1, D3, D4 e D5 sono riassunti in modo sommario.
- **Blocco 2 – numero della delibera**: il testo del documento è la deliberazione del Commissario con i poteri del Consiglio **n. 17 del 15/12/2025** (proposta n. 78); il nome del file e CLAUDE.md dicevano "n. 15". Quale numero va citato nei pareri? Proposta: n. 17 (dal testo firmato).
- **Blocco 2 – zone A-F e ambiti PUC**: la delibera calcola K3 per zone omogenee A-F, ma il PUC di Ottaviano ragiona per ambiti. Serve una tabella di corrispondenza ambito → zona (da te o da altro atto)? Senza, il parere indicherà il K3 solo se la scheda riporta la zona.
- **Blocco 2 – costo di costruzione**: la delibera dà solo Vb = 307,54 €/mq. L'aliquota (5-20%), le maggiorazioni, il non residenziale e il calcolo per edifici esistenti non ci sono: sono nella delibera di Giunta del 14/04/2022 n. 58 o in altro atto? Puoi caricarlo?
- **Blocco 2 – oneri e condoni/sanatorie**: per condoni e sanatorie con riferimento ad anni precedenti, quale tabella si applica (data della domanda, del rilascio, o attuale)? Proposta: segnalare e non calcolare.
- **Blocco 2 – conflitti NTA/RUEC (sez. 8 n. 14)**: ristrutturazione con demolizione-ricostruzione (PdC in Allegato 1, SCIA nel RUEC), impermeabilizzazione (40% delle superfici non edificate contro 60% del lotto), cambio d'uso senza opere. Proposta: prevalgono le NTA solo dove non sono superate dal DPR 380/D.Lgs 222; confermi?
- **Blocco 2 – Allegato 1**: il titolo del file dice "PUC adottato": va considerato coincidente con l'allegato del testo approvato (Decreto 484/2015)? Il testo è stato letto con strato di testo selezionabile; i refusi di impaginazione non toccano i numeri riportati.
- **Blocco 2 – K3**: la tabella (sez. 29.3) è ricostruita dal testo estratto, dove il grassetto non si vede: va controllata sul PDF originale prima di citare un coefficiente; in particolare i valori 0,91 e 0,83 della zona F per la ristrutturazione.
- **Blocco 3 – Allegato 1 L. 326/2003 (OCR)**: il PDF aveva il font corrotto ed è stato letto con OCR. Puoi controllare sull'originale il rinvio della tipologia 6 (nel testo OCR "art. 3 c. 1 lettera 5)": quasi certamente lett. b, manutenzione straordinaria)? E ci puoi fornire il modello di domanda con le tabelle 1, 3, 4, C e D, oppure confermi che non servono (oblazione non calcolata)?
- **Blocco 3 – tipologie 4-6 e vincolo paesaggistico**: poiché tutto il territorio di Ottaviano è vincolato, le tipologie 4-6 rientrano nel c. 26 lett. a (immobili vincolati) e quindi sono sanabili a condizione del parere dell'autorità del vincolo? La LR 10/2004 nel testo ricevuto non dice nulla. Proposta: segnalare e lasciare la decisione al funzionario.
- **Blocco 3 – soglia dei 450 mc**: perizia giurata e idoneità statica solo oltre 450 mc (Allegato 1, L. 47/85 art. 35) o sempre (LR 10/2004 art. 5)? Proposta: richiederle sempre in sede di parere come dato mancante, citando entrambe le fonti.
- **Blocco 3 – tipologia 2**: serve la data di riferimento della conformità urbanistica ("data di entrata in vigore del presente provvedimento"), che il PDF non riporta. Va ricavata dal testo dell'art. 32 L. 326/2003 / del decreto-legge di conversione: puoi indicarla o fornire il testo?
- **Blocco 3 – DPCM 12/12/2005**: (a) l'art. 4 lega l'entrata in vigore a un accordo con la direzione regionale del Ministero: risulta stipulato in Campania, e hai atti regionali che integrano la relazione (art. 3)? (b) per le opere dell'Allegato B del DPR 31/2017 vale la relazione semplificata dell'Allegato D DPR 31/2017 e non la scheda del DPCM: confermi? (c) il RUEC recepisce l'elenco di opere "a documentazione semplificata" (nota 2 della scheda)? Non risulta dalla sez. 6.
- **Blocco 3 – verifica della qualità della relazione paesaggistica**: confermi che il parere si limiti a verificare la presenza degli elementi minimi (campo L della sez. 10) e non a valutarne la qualità (compito di Commissione per il paesaggio e Soprintendenza)?
