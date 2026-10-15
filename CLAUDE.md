# CLAUDE.md – Progetto "Valutazione Pratiche Urbanistiche" (Comune di Ottaviano)

Aggiornato al 15/10/2026. Leggi questo file all'inizio di ogni sessione. Rispondi sempre in italiano.

## 1. Chi e cosa
- **Utente**: Milvio Nocerino, istruttore tecnico (cat. C, in attesa di D), Settore Urbanistica ed Edilizia Privata, Comune di Ottaviano (NA). Lavora da solo in ufficio.
- **Obiettivo**: automatizzare la parte oggettiva dell'istruttoria delle pratiche edilizie (PdC, SCIA, CILA, condoni, agibilità; **esclusi** CDU e accessi agli atti). Il sistema legge una **scheda anonima** (solo dati oggettivi, nessun dato personale), la confronta con la normativa ed emette un **parere di fattibilità positivo/negativo** con motivazione, riferimenti normativi precisi e, se negativo, **alternative**. Output: PDF con grafica istituzionale del Comune.
- **Decisione e firma restano al funzionario**: il PDF deve dirlo. Il parere è istruttoria di supporto.
- **Avvio**: a comando (pulsante "valuta pratica" nell'app dell'ufficio) o a intervalli. Una scheda già valutata **non va rivalutata** (cartelle Drive: *pratiche/Da valutare, pratiche/Valutate, pratiche/Pareri*).
- **Cartelle**: vanno create **sempre** in Google Drive, `H:\Il mio Drive` ("Il mio Drive"), nella cartella di progetto `Valutazione Pratiche Urbanistiche` (vedi sezione 3 bis).
- **API a pagamento escluse per ora**: si testa dentro Claude (Project, flusso semi-manuale). Poi, se serve, si valuta l'API.
- Lo stesso lavoro sarà fatto anche con ChatGPT per confrontare i pareri.

## 2. Regole di lavoro (vincolanti)
1. **Costi**: è la linea guida di ogni scelta. Suggerisci quando cambiare modello o chat. Una chat per pratica. Sonnet per l'istruttoria di routine; Opus solo per casi dubbi o per la revisione finale della sintesi.
2. **Non inventare norme**. Cita solo articoli presenti nei testi forniti o nella sintesi. Se un riferimento manca, dillo e chiedi il testo.
3. Dato mancante nella scheda: **elencalo, non supporlo**.
4. Quando due fonti sono in conflitto, **segnala il conflitto** e non risolverlo da solo. Eccezione decisa dall'utente: **tra NTA e RUEC prevalgono le NTA** (es. parcheggi: si applica NTA art. 29 bis = 1 posto, non RUEC art. 36 c. 4 = 2 posti; nel parere citare entrambe e indicare la prevalenza).
5. Grafica Comune: blu #1F3864, #2E5395, #4A7FB5; teal #2A9D8F (poco); testo #262626; grigio #6B6B6B; sfondo #F2F4F7. Word: Aptos. Web: Inter o system-ui. Bordi arrotondati 8px. Inserire lo **stemma** del Comune quando l'utente lo fornisce (non ancora ricevuto).

## 3. File del progetto
- `sintesi-normativa-ottaviano.md` (**v1.5, ~750 righe**): sintesi operativa di tutte le norme lette. È la base di ogni parere. Copia anche nel Project Claude.ai come `claude/sintesi-normativa-ottaviano.md`.
- `brief-progetto-valutazione-pratiche.md`: brief iniziale.
- Testi sorgente (PDF originali, da conservare in una cartella `norme/`): NTA PUC (Decr. 484 del 03/11/2015), RUEC (giugno 2015), PSAI Campania Centrale (Delib. C.I. n. 1 del **23/02/2015**) con Allegati A, C, E; L. 47/85, art. 39 L. 724/94, art. 32 L. 326/03; DPR 380/01 (ed. 28/07/2026); DPR 31/2017 (ed. 2.0); D.Lgs 42/04 (versione aggiornata, 51 pagine); D.Lgs 222/2016; LR 16/2004 (con LR 5/2024); LR 21/2003.
- Indice della sintesi: 1 gerarchia fonti; 2 sequenza di verifica; 3 ambiti PUC; 4 vincoli; 5 e 5 bis PSAI; 6 RUEC; 7 titoli; 8 conflitti; 9 testi mancanti; 10 campi scheda; 11 formato parere; 12 condoni; 13 paesaggio (D.Lgs 42 e DPR 31); 14 DPR 380; 15 D.Lgs 222; 16 LR 16/2004; 17 LR 21/2003.

**Nota sui PDF**: il D.Lgs 42/04 aggiornato e il D.Lgs 222/2016 hanno un font corrotto e sono stati letti con **OCR** (tesseract, solo inglese: accenti imprecisi). Prima di citare un comma in un parere ufficiale, rileggere il PDF originale.

## 3 bis. Struttura cartelle (Google Drive, "Il mio Drive")
```
Valutazione Pratiche Urbanistiche/
├── docs/              sintesi normativa, brief, questo CLAUDE.md
├── norme/
│   ├── comunali/      NTA PUC (+ Allegato 1), RUEC, tabelle oneri e costo di costruzione
│   ├── psai/          PSAI 2015 + Allegati A, B, C, D, E
│   ├── statali/       DPR 380, DPR 31, D.Lgs 42, D.Lgs 222 (+ Tabella A, glossario), L. 47/85, L. 724/94, L. 326/03 (+ all. 1), DPCM 12/12/2005
│   └── regionali/     LR 16/2004, LR 21/2003, LR 13/2022, LR 15/2000, LR 19/2009, LR 10/2004, RR 3/2025, RR 5/2011
├── scheda/            modello scheda anonima
├── project-claude/    istruzioni del Project
├── modelli-parere/    modello PDF del parere, stemma
├── pratiche/          Da valutare / Valutate / Pareri
└── pratiche-test/     pratiche reali anonimizzate per i test
```

## 4. Punti chiave già emersi (da non perdere)
- **Tutto il territorio di Ottaviano è vincolato paesaggisticamente** (art. 134 lett. b/c D.Lgs 42/04; parco e vulcani art. 142 lett. f, l). La data di istituzione di ogni vincolo rispetto alla data dell'opera è un **dato obbligatorio della scheda**.
- **LR 21/2003 (zona rossa vesuviana)**: vieta titoli per nuova edilizia residenziale e cambi d'uso verso l'abitativo; un intervento che aumenta volumi abitabili o carico urbanistico è non fattibile. **Ottaviano è in zona rossa** (confermato dall'utente).
- **Classificazione sismica**: Ottaviano è in **zona sismica 2** (confermato dall'utente).
- **Piano comunale di emergenza (protezione civile)**: approvato, pubblicato su https://www.comune.ottaviano.na.it/it/page/piano-comunale-protezione-civile-2 (testo da leggere).
- **Art. 33 quater LR 16/2004 c. 21 lett. g**: incentivi di rigenerazione esclusi in zona ad alto rischio vulcanico e in aree a rischio idraulico o geomorfologico elevato/molto elevato.
- **PSAI**: la versione 2015 è **quella vigente** (confermato). Classi R1-R4; Allegato C (matrici E, V, D, P); sanatoria rigettata per R3/R4 e quote incompatibili (Allegato A §4). **Decisione dell'utente: la pericolosità PSAI non va considerata**; si valuta solo il rischio.
- **Sanatoria ordinaria**: art. 36 DPR 380/01 = doppia conformità; art. 36-bis = conformità urbanistica alla domanda, edilizia alla realizzazione. Indicare sempre quale si applica.
- **Sanatoria paesaggistica**: art. 167 c. 4 D.Lgs 42/04 non ammette compatibilità per opere con nuovi volumi/superfici; art. 36-bis c. 4 DPR 380 prevede il parere anche in quel caso. **I testi non coincidono**: segnalare, non concludere in automatico.
- **Tolleranze** (art. 34-bis): 2% generale; soglie più alte per opere entro il 24/05/2024 e unità piccole. Servono data di realizzazione e superficie utile assentita.
- **Condoni**: tre leggi con date e limiti diversi (sezione 12). Manca l'allegato 1 L. 326/03. Tabella oblazione L. 47/85 e legge campana attuativa **non servono** (escluse dall'utente).

## 5. Ancora da fare / testi mancanti
1. **Testi sorgente su Drive** (account `valutazioni.urbanistiche@gmail.com`, condiviso con l'account di lavoro): cartella `norme` https://drive.google.com/drive/folders/1lw4KAKV9YelH1nVNmxR6PzANryaArteB e cartella `piano emergenza` https://drive.google.com/drive/folders/1fRBr6yUj9WxLxe9H46axVrKnoo_iG3Z8.
   **Ricevuti il 15/10/2026, da leggere e inserire nella sintesi**: RR 3/2025 (`2106_3_2025RRStorico.pdf`); LR 13/2022; LR 15/2000; LR 19/2009; LR 10/2004; RR 5/2011; PSAI Allegati B e D; Allegato 1 NTA (indici urbanistici); allegato del D.Lgs 222/2016 (`2016_0222_allegato.pdf`, 80 MB, presumibilmente Tabella A); allegato 1 L. 326/03; delibera n. 15 del 15/12/2025 (oneri e costo di costruzione); piano di protezione civile (decreto COC 17/2024, elaborati A e B).
   **Ancora mancanti**: glossario unico (D.M. 2/3/2018); DPCM 12/12/2005; DPR 31/2017 (non presente nella cartella); file `DPR-380-...28-07-2026.pdf` è corrotto (90 byte, testo semplice): ricaricarlo; stemma del Comune; `sintesi-normativa-ottaviano.md` e `brief-progetto-valutazione-pratiche.md` (non presenti su Drive).
   **Esclusi dall'utente (non servono)**: nota del Comune, Piano del Parco, PTP, Piano Paesaggistico Regionale, tabella oblazione L. 47/85, legge campana attuativa del condono.
2. **Domande aperte all'utente**: stemma del Comune. (Chiuse il 15/10/2026: NTA prevalgono sul RUEC; PSAI 2015 vigente; pericolosità PSAI esclusa; zona rossa sì; zona sismica 2; piano di emergenza esistente, link in sez. 4.)
3. **Scheda anonima**: ridurla ai campi essenziali partendo dalla sezione 10 della sintesi e dai campi aggiuntivi delle sezioni 13.8, 14.8, 17.1; creare il modello compilabile (Excel o modulo) e testarlo con pratiche vere anonimizzate.
4. **Istruzioni del Project**: scrivere il prompt di sistema dell'agente (sequenza di verifica sez. 2, formato parere sez. 11, regole del punto 2 di questo file).
5. **Modello del PDF del parere** con grafica istituzionale e dicitura sulla firma del funzionario.
6. **Flusso Drive** (Da valutare / Valutate / Pareri) e pulsante "valuta pratica" nell'app. Solo dopo che schede e pareri funzionano a mano.
7. **Revisione finale** della sintesi con Opus (una volta sola), dopo l'arrivo dei testi mancanti.

## 6. Come proseguire (suggerito)
- In questa chat/sessione: aggiornare la sintesi con ogni nuovo testo ricevuto (nuova sezione numerata, aggiornare la sezione 9 e la versione nell'intestazione).
- Per ogni testo nuovo: estrarre il testo (`pdftotext -layout`; se esce illeggibile, OCR con tesseract su pagine rasterizzate a ~170 dpi), leggere solo gli articoli utili, verificare i riferimenti incrociati.
- Chiudere ogni aggiornamento con: cosa è cambiato, cosa manca ancora, eventuali conflitti da decidere.
- Suggerire all'utente un cambio modello o chat quando il compito passa da lettura/sintesi (Sonnet) a revisione critica (Opus) o a una pratica singola (nuova chat).
