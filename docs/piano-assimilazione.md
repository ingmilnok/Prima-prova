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
| 1 | Sonnet | PSAI Allegato B (`1l1R2FriNXLpngVNTUjQhLOs3bb8kJQId`, `ALL_ B_COMP_GEOL_ 2015.pdf`) e Allegato D (`1beAMdeR5zTfvTLBohINQ3QhEl5iumEwK`, `ALL_D_TOTALE_2015.pdf`, 7,8 MB) | 27 | da fare |
| 2 | Sonnet | Allegato 1 NTA, indici urbanistici (`1E_IMCn93RqpKdExSkUUd69ISVnjm1vTw`) e delibera n. 15 del 15/12/2025 su oneri e costo di costruzione (`1022gFmiMhprY0WC4Yy6swcdgwbkFOyWE`) | 28, 29 (+ sez. 3 e 7 se cambiano) | da fare |
| 3 | Sonnet | Allegato 1 L. 326/2003 (`1FOGVegFuV6EttElXP4-xTSTuVmcd3Pz9`) e DPCM 12/12/2005 relazione paesaggistica (`1woRfD3L6Wv_3P1EIeq7ROcOXjdvk7BWf`) | 30 (+ aggiornare sez. 12 e 21), 31 (+ sez. 13) | da fare |
| 4 | Sonnet | Allegato D.Lgs 222/2016 con la Tabella A (`1XP_dzuHaGwot1FihSSDpU2KE9gFf8L-p`, 19 MB, probabile scansione: OCR) e glossario unico DM 2/3/2018 (`1beL2Vu5QRVkTDxIZBl3Z5ZTykS5j_HB0`) | 32, 33 (+ sez. 15) | da fare |
| 5 | Sonnet | Piano di protezione civile: cartella `piano emergenza` (`1fRBr6yUj9WxLxe9H46axVrKnoo_iG3Z8`; decreto COC 17/2024, elaborati A e B) | 34 (+ sez. 5 bis e 17 se serve) | da fare |
| 6 | Opus | Revisione finale critica della sintesi, **una volta sola**: coerenza interna, riferimenti incrociati, conflitti, numeri di articolo, sezione 9 ripulita, passi per le istruzioni del Project. Scrivere le correzioni piccole nel file; le questioni grandi nelle domande per l'utente | tutte | da fare |

Dopo il blocco 6 restano i punti 3-6 di CLAUDE.md sez. 5 (scheda anonima, istruzioni del Project, modello PDF del parere, flusso Drive): non fanno parte dell'assimilazione.

## Domande per l'utente (le sessioni le aggiungono qui)
- (nessuna per ora)
