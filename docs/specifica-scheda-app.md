# Specifica della scheda anonima – per la chat di creazione dell'app

**Da incollare nella chat in cui si costruisce l'app dell'ufficio (Comune di Ottaviano – Urbanistica ed Edilizia Privata).** Documento autosufficiente: descrive *che tipo di scheda* l'app deve implementare (campi, elenchi, regole di obbligatorietà, controlli, uscita). Versione 1.0 del 20/10/2026, derivata da `scheda/scheda-anonima-v1.xlsx` (sintesi normativa v2.8). Rispondere sempre in italiano.

## 1. Scopo e principi

- La scheda raccoglie **solo dati oggettivi e anonimi** di una pratica edilizia (PdC, SCIA, SCIA alternativa al PdC, CILA, edilizia libera, condoni L. 47/85, L. 724/94, L. 326/03, agibilità, accertamenti di conformità art. 36 e 36-bis DPR 380/01). **Esclusi** CDU e accessi agli atti.
- **Privacy**: nessun dato personale (nomi, CF, indirizzi, numeri di protocollo identificativi, particelle catastali complete). Identificativo = `A01 Codice pratica anonimo`, scelto dall'ufficio. L'app non deve chiedere né memorizzare dati personali.
- La scheda viene letta da un agente (Project Claude) che la confronta con la sintesi normativa ed emette un **parere di fattibilità positivo/negativo** motivato, con riferimenti normativi, alternative se negativo. **Decisione e firma restano al responsabile del procedimento**: ogni parere/PDF riporta la dicitura *«Istruttoria di supporto: la decisione e la firma restano al responsabile del procedimento»*.
- **Dato mancante = elencato, mai supposto.** Vuoto o «Non noto» in un campo richiesto → compare nella lista dei mancanti.
- Una scheda già valutata **non va rivalutata** (stato pratica: Da valutare → Valutata → Parere emesso).
- Costi: nessuna API a pagamento per ora; l'app deve funzionare in locale/Drive e produrre un file (JSON/CSV/Markdown) che si incolla o carica nel Project Claude.

## 2. Modello dati

- **Una scheda = un insieme di 221 righe-campo** con colonne stabili: `Codice | Sezione | Campo | Valore | Unità | Obbligatorio per | Nota | Stato`. I codici (A01, B05 …) **non cambiano** tra versioni 1.x; nuovi campi ricevono un nuovo codice in coda al blocco.
- Tipi: `elenco` (menu a tendina da una lista chiusa, con «Non noto» ove previsto), `numero`, `data`, `testo`, `costante` (precompilata, non modificabile), `calcolato` (formula).
- **Gruppi di procedimento** (da A02 tramite lista «Tipo di procedimento»): `ORD` = PdC, SCIA, SCIA alternativa al PdC, CILA, Edilizia libera; `CON` = Condono L. 47/85 / 724/94 / 326/03; `AGI` = Agibilità; `ACC` = Accertamento di conformità art. 36 / art. 36-bis.
- **Flag della qualificazione dell'intervento** (A03, lista «Qualificazione»): `P` = ha parametri edilizi (volumi, sagoma, superfici); `C` = può cambiare uso o carico urbanistico; `DR` = demolizione e ricostruzione; `T` = intervento «tetto» (sottotetto). Una regola `ORD:P` significa: richiesto per i titoli ordinari **solo se** la qualificazione scelta ha il flag P.
- **Regola di obbligatorietà per campo**: per ciascun gruppo (ORD/CON/AGI/ACC) il valore è `X` (sempre), `P`/`C`/`DR`/`T` (solo se la qualificazione ha quel flag), oppure vuoto (non richiesto). Un campo è effettivamente richiesto se: (a) la regola del gruppo corrente è soddisfatta, **e** (b) la sua **condizione** è vera, **e** (c) il suo **interruttore di sezione** è «Sì» (se la sezione ne ha uno).
- **Interruttori «Sezione … pertinente?» (Sì/No)** per i blocchi H, I, K, L, M, N, S, P. Con «No» i campi del blocco sono disattivati (non richiesti, righe grigie). Nessun interruttore è precompilato: ognuno è obbligatorio.
- **Condizione**: formula di visibilità/obbligo aggiuntiva per alcuni campi (nelle tabelle sotto in sintassi Excel; `Elenchi!$J$2:$N$40` = tabella «Ambito PUC» di §6, colonne: ambito, art., zona default, colonna K3, agricolo; `{COD}` = valore del campo COD; 1 = attivo). L'app può riscriverla nel proprio linguaggio.

## 3. Interfaccia richiesta

1. Compilazione a **moduli per sezione** (Z sola lettura; A–P), con menu a tendina dalle liste di §6, validazione tipo (numero/data), unità accanto al campo e **nota** come aiuto contestuale.
2. Ricalcolo immediato: campi non pertinenti nascosti o grigi; contatore **richiesti / compilati / mancanti / non noti** (rosso se mancanti > 0).
3. Pulsante **«Valuta pratica»**: consentito anche con mancanti (il parere li elencherà), ma con avviso.
4. **Esportazione** della scheda in forma univoca per l'agente: Markdown (`Codice | Campo | Valore | Unità`, solo campi richiesti e compilati, + sezione «Mancanti» + sezione «Segnalazioni automatiche») e JSON equivalente.
5. Grafica istituzionale: blu #1F3864, #2E5395, #4A7FB5; teal #2A9D8F (poco); testo #262626; grigio #6B6B6B; sfondo #F2F4F7; font Inter o system-ui; bordi arrotondati 8 px; **stemma** `docs/stemma-comune-ottaviano.png` nell'intestazione.
6. Cartelle (Google Drive «Il mio Drive» → `Valutazione Pratiche Urbanistiche/pratiche/`): `Da valutare`, `Valutate`, `Pareri`.

## 4. Campi calcolati e logica speciale

- **B02 Zona A-F di default**: dedotta dall'ambito PUC (B01) tramite la tabella di §6 (colonne «Zona A-F di default», «Colonna K3»). È un'assimilazione (sintesi sez. 35.0), non una norma. Nessun default per art. 17, artt. 32-35 e «Altro/non noto».
- **B03 Zona indicata dal funzionario** (opzionale): **prevale** su B02.
- **B04 Zona applicata** = B03 se compilata, altrimenti B02. Se vuota → segnalazione «zona non determinata per il K3».
- **E16, H03**: celle calcolate (vedi formule nelle tabelle di §5).
- Ambito agricolo (flag 1/0 nella tabella ambiti): attiva campi specifici (es. D22 piano di sviluppo aziendale per nuova costruzione agricola).

## 5. Campi

### Dati noti per Ottaviano
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Z01 | Zona sismica | costante | 2 |  |  |  |  |  |  |  | Sez. 14.7, 24 (conferma dell'utente) |
| Z02 | Zona rossa vesuviana (LR 21/2003) | costante | Sì – Ottaviano compreso |  |  |  |  |  |  |  | Sez. 17; la LR 21/2003 resta in vigore (PPR non in vigore: sez. 8 n. 28) |
| Z03 | Vincolo paesaggistico | costante | Intero territorio (art. 134 lett. b/c; art. 142 lett. f, l D.Lgs 42/04; D.M. 02/09/1961) |  |  |  |  |  |  |  | Sez. 13, 37.1 |
| Z04 | PSAI vigente | costante | Versione 2015 (Delib. C.I. n. 1 del 23/02/2015): si valuta il solo rischio |  |  |  |  |  |  |  | Sez. 5, 5 bis; CLAUDE.md sez. 4 |
| Z05 | Funzioni sismiche | costante | Commissione sismica comunale attiva |  |  |  |  |  |  |  | Sez. 36; sez. 8 n. 28 |
| Z06 | Delibera oneri e K2 | costante | Delibera n. 17 del 15/12/2025; K2 = 0,59 |  |  |  |  |  |  |  | Sez. 29, 8 n. 22 e 27 |
| Z07 | Prevalenza NTA / RUEC | costante | Tra NTA e RUEC prevalgono le NTA |  |  |  |  |  |  |  | CLAUDE.md sez. 2 punto 4; sez. 8 n. 1 e 27 |

### Procedimento
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| A01 | Codice pratica anonimo (scelto dall'ufficio, senza nomi né numeri di protocollo identificativi) | testo |  |  | X | X | X | X |  |  | Sez. 10 A |
| A02 | Tipo di procedimento | elenco | Tipo di procedimento |  | X | X | X | X |  |  | Sez. 10 A1; 7, 12, 14, 32 |
| A03 | Qualificazione dell'intervento (DPR 380/2001) | elenco | Qualificazione dell'intervento |  | X |  |  | X |  |  | Sez. 10 A2; 14.1, 28.3, 32. Per i condoni vale la tipologia di illecito (sez. H) |
| A04 | Descrizione sintetica delle opere (solo dati tecnici, nessun dato personale) | testo |  |  | X | X |  | X |  |  | Sez. 10 A3 |
| A05 | Data di presentazione della domanda / del titolo | data |  |  | X | X | X | X |  |  | Sez. 10 A4; 14.2; per la SCIA/CILA decorrenza della delibera oneri (29.1) |
| A06 | Data di rilascio del PdC (solo PdC; decorrenza della delibera oneri) | data |  |  | X |  |  |  |  | `IF({A02}="PdC",1,0)` | Sez. 10 A4; 29.1 |

### Localizzazione urbanistica e vincoli
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| B01 | Ambito PUC (articolo NTA) | elenco | Ambito PUC (articolo NTA) |  | X | X |  | X |  |  | Sez. 3, 28, 35.0 |
| B02 | Zona omogenea A-F di default (calcolata dall'ambito) | calcolato | `IFERROR(VLOOKUP({B01},Elenchi!$J$2:$N$40,3,FALSE)&"","")` |  |  |  |  |  |  |  | Sez. 35.0 (assimilazione, non norma). Cella calcolata: non compilare |
| B03 | Zona omogenea A-F, se nota (prevale sul default dell'ambito) | elenco | ZONA |  |  |  |  |  |  |  | Sez. 35.0 e 35.6: la zona della scheda prevale |
| B04 | Zona omogenea applicata (calcolata) | calcolato | `IF({B03}<>"",{B03},{B02})` |  |  |  |  |  |  |  | Cella calcolata: zona della scheda, altrimenti default dell'ambito. Se vuota e serve il K3, il parere la elenca come dato mancante |
| B05 | Zona PTP | elenco | PTP |  | X | X |  | X |  |  | Sez. 4; NTA art. 3 c. 2-3 |
| B06 | Rapporto con il Parco Nazionale del Vesuvio | elenco | PARCO |  | X | X |  | X |  |  | Sez. 4, 32.2 (nulla osta Ente Parco) |
| B07 | Interessa SIC/ZPS | elenco | SN |  | X | X |  | X |  |  | Sez. 4 |
| B08 | Area boscata | elenco | SN |  | X | X |  | X |  |  | Sez. 4 |
| B09 | Area archeologica | elenco | SN |  | X | X |  | X |  |  | Sez. 4 |
| B10 | Immobile bene culturale (art. 10 D.Lgs 42/04) o soggetto a tutela indiretta (artt. 45-47) | elenco | SN |  | X | X |  | X |  |  | Sez. 10 B7 e B10; 13.5, 13.8 |
| B11 | Fascia di rispetto interessata | elenco | FASCIA |  | P | X |  | P |  |  | Sez. 4; 8 n. 2 (fasce stradali). Se più di una, indicarle nella descrizione |
| B12 | Distanza dalla fascia / dall'elemento vincolante | numero |  | m | P |  |  | P |  | `IF(AND({B11}<>"",{B11}<>"Nessuna",{B11}<>"Non noto"),1,0)` | Sez. 10 B8 |
| B13 | Vincolo paesaggistico rafforzato (provvedimento specifico oltre al vincolo generale) | elenco | SN |  | X | X |  | X |  |  | Sez. 13.8 |
| B14 | Data di istituzione del vincolo paesaggistico applicabile all'immobile | data |  |  |  | X |  | X |  |  | OBBLIGATORIO: CLAUDE.md sez. 4; sez. 12.3, 12.9. Va confrontata con la data delle opere (Controlli) |
| B15 | Data di istituzione del vincolo del Parco (o «non applicabile» se fuori Parco) | testo |  |  |  | X |  | X |  |  | OBBLIGATORIO: sez. 10 B11 |
| B16 | Data di istituzione di altri vincoli presenti (idrogeologico, archeologico, ecc.; o «non applicabile») | testo |  |  |  | X |  | X |  |  | OBBLIGATORIO: sez. 10 B11 |
| B17 | Allegato A o B del DPR 31/2017 applicabile | elenco | DPR31 |  | X |  |  | X |  |  | Sez. 13.6, 13.8 |
| B18 | Voce dell'Allegato (es. B.16) | testo |  |  | X |  |  | X |  | `IF(OR({B17}="Allegato A (esclusa)",{B17}="Allegato B (semplificata)"),1,0)` | Sez. 13.6 |

### Rischi
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| C01 | Rischio idraulico (PSAI) | elenco | RISCHIO |  | X | X |  | X |  |  | Sez. 5, 5 bis; 10 C13 |
| C02 | Rischio frana (PSAI) | elenco | RISCHIO |  | X | X |  | X |  |  | Sez. 5; 10 C14 (R3/R4 anche per art. 33 quater LR 16/2004) |
| C03 | Edificio in area a rischio | elenco | INAREA |  | X | X |  | X |  | `IF(OR(AND({C01}<>"",{C01}<>"Nessuno",{C01}<>"Non noto"),AND({C02}<>"",{C02}<>"Nessuno",{C02}<>"Non noto")),1,0)` | Sez. 10 C15 |
| C04 | Presenza di cavità o sinkholes | elenco | SN |  | P |  |  | P |  |  | Sez. 10 C16 |
| C05 | Distanza da alvei/argini | numero |  | m | P |  |  | P |  |  | Sez. 10 C16 |
| C06 | Rischio vulcanico/sismico segnalato dalle carte del PUC (P4.1, P4.3) | elenco | SN |  | P |  |  | P |  |  | Sez. 10 C17; NTA art. 16 c. 2 |
| C07 | Lavori su parti strutturali | elenco | SN |  | X |  |  | X |  |  | Sez. 14.7, 24 |
| C08 | Categoria dell'intervento ai fini sismici | elenco | CATSISM |  | X |  |  | X |  | `IF({C07}="Sì",1,0)` | Sez. 24 |

### Stato legittimo e difformità
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| D01 | Titolo originario dell'edificio | elenco | TITORIG |  | X |  | X | X |  |  | Sez. 10 D19; 14.8 |
| D02 | Data del titolo originario (nessun numero di protocollo) | data |  |  | X |  | X | X |  | `IF(AND({D01}<>"",{D01}<>"Non noto",LEFT({D01},6)<>"Nessun"),1,0)` | Sez. 10 D19 |
| D03 | Epoca di costruzione | elenco | EPOCA |  | X | X |  | X |  |  | Sez. 3 (art. 15 c. 8: ante 1945/1955), 14.8 |
| D04 | Edificio già oggetto di condono rilasciato | elenco | SN |  | X |  | X | X |  |  | Sez. 10 D19; 12 |
| D05 | Stato legittimo | elenco | STATOLEG |  | X |  |  | X |  |  | Sez. 14.8, 26 (LR 31/2021 art. 27 c. 4: «da acquisire d'ufficio») |
| D06 | Difformità presenti rispetto allo stato legittimo | elenco | SN |  | X | X | X | X |  |  | Sez. 10 D20; 14.4 |
| D07 | Qualificazione proposta della difformità | elenco | DIFFQUAL |  | X | X | X | X |  | `IF({D06}="Sì",1,0)` | Sez. 14.4 |
| D08 | Data di realizzazione dell'abuso / delle opere difformi | data |  |  |  | X |  | X |  |  | Sez. 10 D21; 12.1, 14.5 |
| D09 | Data di ultimazione delle opere | data |  |  |  | X | X | X |  |  | Sez. 10 D21; 12.1, 14.3 |
| D10 | Documentazione che prova la data | elenco | DOCDATA |  |  | X |  | X |  |  | Sez. 14.8; 12.5 |
| D11 | Superficie utile assentita dell'unità | numero |  | mq |  |  |  | X |  | `IF({D06}="Sì",1,0)` | Tolleranze art. 34-bis: sez. 14.5 |
| D12 | Volume assentito | numero |  | mc |  |  |  | X |  | `IF({D06}="Sì",1,0)` | Sez. 14.5, 14.8 |
| D13 | Volume realizzato | numero |  | mc |  |  |  | X |  | `IF({D06}="Sì",1,0)` | Sez. 14.5, 14.8 |
| D14 | Superficie coperta assentita | numero |  | mq |  |  |  | X |  | `IF({D06}="Sì",1,0)` | Sez. 14.5 |
| D15 | Superficie coperta realizzata | numero |  | mq |  |  |  | X |  | `IF({D06}="Sì",1,0)` | Sez. 14.5 |
| D16 | Altezza assentita | numero |  | m |  |  |  | X |  | `IF({D06}="Sì",1,0)` | Sez. 14.5 |
| D17 | Altezza realizzata | numero |  | m |  |  |  | X |  | `IF({D06}="Sì",1,0)` | Sez. 14.5 |
| D18 | Distacco minimo assentito | numero |  | m |  |  |  | X |  | `IF({D06}="Sì",1,0)` | Sez. 14.5 |
| D19 | Distacco minimo realizzato | numero |  | m |  |  |  | X |  | `IF({D06}="Sì",1,0)` | Sez. 14.5 |
| D20 | Titoli o domande sulla stessa unità negli ultimi 5 anni (elenco: tipo e anno, senza estremi) | testo |  |  | P |  |  |  |  |  | Allegato 1 NTA art. 8 c. 2: sez. 28 |
| D21 | Uso agricolo del fondo documentato | elenco | SN |  | X | X |  | X |  | `IF(IFERROR(VLOOKUP({B01},Elenchi!$J$2:$N$40,5,FALSE),0)=1,1,0)` | NTA artt. 7-11: sez. 3 |
| D22 | Piano di sviluppo aziendale presentato (campo aggiunto) | elenco | SN |  | X |  |  |  |  | `IF(AND(IFERROR(VLOOKUP({B01},Elenchi!$J$2:$N$40,5,FALSE),0)=1,{A03}="Nuova costruzione"),1,0)` | Campo aggiunto rispetto alla sez. 10: NTA artt. 8, 9, 11 (sez. 3) |

### Parametri edilizi
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| E01 | Superficie fondiaria (S.F.) | numero |  | mq | P |  |  | P |  |  | Allegato 1 NTA: sez. 28 |
| E02 | Superficie coperta (S.C.) – stato di fatto | numero |  | mq | P |  |  | P |  |  | Sez. 28 |
| E03 | Superficie coperta (S.C.) – progetto | numero |  | mq | P |  |  | P |  |  | Sez. 28 |
| E04 | Superficie utile (S.U.) – stato di fatto | numero |  | mq | P |  |  | P |  |  | Sez. 28 |
| E05 | Superficie utile (S.U.) – progetto | numero |  | mq | P |  |  | P |  |  | Sez. 28 |
| E06 | Volume (V.C.) – stato di fatto | numero |  | mc | P |  |  | P |  |  | Sez. 28 |
| E07 | Volume (V.C.) – progetto | numero |  | mc | P |  |  | P |  |  | Sez. 28 |
| E08 | Altezza (H.F./H.M.) – stato di fatto | numero |  | m | P |  |  | P |  |  | Sez. 28 |
| E09 | Altezza (H.F./H.M.) – progetto | numero |  | m | P |  |  | P |  |  | Sez. 28 |
| E10 | Numero piani (N.P.) – stato di fatto | numero |  | n. | P |  |  | P |  |  | Sez. 28 |
| E11 | Numero piani (N.P.) – progetto | numero |  | n. | P |  |  | P |  |  | Sez. 28 |
| E12 | Unità immobiliari – stato di fatto | numero |  | n. | C |  |  | C |  |  | Sez. 10 E25 |
| E13 | Unità immobiliari – progetto | numero |  | n. | C |  |  | C |  |  | Sez. 10 E25 |
| E14 | Volumetria originaria | numero |  | mc | P | X |  | P |  |  | Sez. 10 E26; 13.8; 12 (ampliamenti %) |
| E15 | Incremento proposto / eseguito | numero |  | mc | P |  |  | P |  |  | Sez. 10 E26 |
| E16 | Incremento sulla volumetria originaria (calcolato) | calcolato | `IF(AND(ISNUMBER({E14}),ISNUMBER({E15}),{E14}>0),{E15}/{E14},"")` | % |  |  |  |  |  |  | Cella calcolata: non compilare |
| E17 | Incrementi già eseguiti negli ultimi 5 anni | numero |  | mc | P |  |  |  |  |  | Sez. 13.8; Allegato 1 NTA |
| E18 | Aumento di carico insediativo | elenco | SN |  | C |  |  | C |  |  | Sez. 10 E27; 17.1 (zona rossa), 5 (PSAI) |
| E19 | Entità dell'aumento (abitanti/addetti/utenti) | numero |  | n. | C |  |  | C |  | `IF({E18}="Sì",1,0)` | Sez. 10 E27 |
| E20 | Aumento di volumi abitabili o del numero di unità abitative | elenco | SN |  | C |  |  | C |  |  | Sez. 17.1 (divieto in zona rossa) |
| E21 | Interrati o seminterrati | elenco | SN |  | P |  |  | P |  |  | Sez. 10 E28; 5 bis (Allegato A3) |
| E22 | Altezza netta dell'interrato/seminterrato | numero |  | m | P |  |  | P |  | `IF({E21}="Sì",1,0)` | Sez. 10 E28 |
| E23 | Quota dell'interrato rispetto al piano campagna | numero |  | m | P |  |  | P |  | `IF({E21}="Sì",1,0)` | Sez. 10 E28 |
| E24 | Quota del primo solaio rispetto al piano campagna | numero |  | m | P |  |  | P |  |  | Sez. 5 bis (Allegato A3 §3) |
| E25 | Distanza minima dai confini | numero |  | m | P |  |  | P |  |  | Sez. 28.2; 6 |
| E26 | Distanza minima da edifici | numero |  | m | P |  |  | P |  |  | Sez. 28.2; 6 |
| E27 | Distanza minima dalle strade | numero |  | m | P |  |  | P |  |  | Sez. 28.2; 6; 8 n. 2 |
| E28 | Pareti finestrate nel fronte verso edifici/confine | elenco | SN |  | P |  |  | P |  |  | Sez. 28.2 (Allegato art. 5) |
| E29 | Larghezza delle parti non finestrate | numero |  | m | P |  |  | P |  | `IF({E28}="No",1,0)` | Allegato 1 NTA art. 5 (limite di 3,00 m): sez. 28.4 n. 4 |
| E30 | Altezza delle parti non finestrate | numero |  | m | P |  |  | P |  | `IF({E28}="No",1,0)` | Allegato 1 NTA art. 5: sez. 28.4 n. 4 |
| E31 | Accordo con il confinante o parete cieca in confine | elenco | SN |  | P |  |  | P |  |  | Sez. 28.2 |
| E32 | Superficie non edificata del lotto | numero |  | mq | P |  |  | P |  |  | Allegato 1 NTA art. 17 (40%): sez. 8 n. 27 (NTA > RUEC) |
| E33 | Superficie già impermeabilizzata – stato di fatto | numero |  | mq | P |  |  | P |  |  | Sez. 28.2, 6 |
| E34 | Superficie impermeabilizzata – progetto | numero |  | mq | P |  |  | P |  |  | Sez. 28.2, 6 |
| E35 | Posti auto pertinenziali previsti | numero |  | n. | C |  |  | C |  |  | NTA art. 29 bis c. 5 (1 posto per unità; RUEC art. 36 c. 4 = 2: prevalgono le NTA): sez. 8 n. 1 |
| E36 | Ubicazione dei posti auto | elenco | PARKUBIC |  | C |  |  | C |  |  | Sez. 17.1; 6 |
| E37 | Distanza dei posti auto dall'unità | numero |  | m | C |  |  | C |  | `IF({E36}="Fuori lotto",1,0)` | Sez. 17.1 |
| E38 | Vincolo pertinenziale dei posti auto | elenco | SN |  | C |  |  | C |  | `IF(AND({E36}<>"",{E36}<>"Non previsti"),1,0)` | Sez. 17.1 |
| E39 | Destinazione d'uso esistente (classi RUEC) | elenco | USO |  | C |  |  | C |  |  | Sez. 6 (classi A-H); 14.1 (art. 23-ter) |
| E40 | Destinazione d'uso di progetto (classi RUEC) | elenco | USO |  | C |  |  | C |  |  | Sez. 6; 14.1 |
| E41 | Mutamento d'uso verso l'abitativo | elenco | SN |  | C |  |  | C |  |  | Sez. 17.1 (zona rossa: vietato, art. 6 c. 2 LR 21/2003) |
| E42 | Superficie di tettoie, autorimesse, volumi tecnici, ricoveri attrezzi | numero |  | mq | P |  |  | P |  |  | Sez. 13.8 |
| E43 | Volume di tettoie, autorimesse, volumi tecnici, ricoveri attrezzi | numero |  | mc | P |  |  | P |  |  | Sez. 13.8 |
| E44 | Demolizione e ricostruzione: corrispondenza di volume, sagoma e sedime | elenco | SN |  | DR |  |  | DR |  |  | Sez. 13.8; 8 n. 27 (SCIA alternativa al PdC) |
| E45 | Intervento «tetto»: incremento di altezza al muro perimetrale | numero |  | m | T |  |  | T |  |  | Allegato 1 NTA art. 14: sez. 28 |
| E46 | Intervento «tetto»: incremento di altezza al colmo | numero |  | m | T |  |  | T |  |  | Sez. 28 |
| E47 | Intervento «tetto»: pendenza delle falde | numero |  | % | T |  |  | T |  |  | Sez. 28 |
| E48 | Intervento «tetto»: larghezza del corpo di fabbrica | numero |  | m | T |  |  | T |  |  | Sez. 28 |
| E49 | Intervento «tetto»: destinazione del volume ottenuto | testo |  |  | T |  |  | T |  |  | Sez. 28 |

### Pareri e titoli già acquisiti
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| F01 | Autorizzazione paesaggistica acquisita | elenco | ACQ |  | P | X | X | X |  |  | Sez. 13.4, 32.2 |
| F02 | Scostamenti dal progetto autorizzato | elenco | SN |  |  |  | X | X |  | `IF({F01}="Acquisita",1,0)` | Sez. 13.8 |
| F03 | Scostamento massimo (su altezza, distacchi, cubatura, superficie coperta, sedime) | numero |  | % |  |  | X | X |  | `IF({F02}="Sì",1,0)` | Sez. 13.8 |
| F04 | Nulla osta Ente Parco | elenco | ACQ |  | X | X |  | X |  | `IF(AND({B06}<>"",{B06}<>"Fuori"),1,0)` | Sez. 32.2; L. 394/1991 art. 13 |
| F05 | Parere della Soprintendenza | elenco | ACQ |  | X | X |  | X |  | `IF({B10}="Sì",1,0)` | Sez. 13.5 |
| F06 | Valutazione di incidenza (SIC/ZPS) | elenco | ACQ |  | X | X |  | X |  | `IF({B07}="Sì",1,0)` | Sez. 10 F37 |
| F07 | Autorizzazione sismica / deposito | elenco | ACQ |  | X |  | X | X |  | `IF({C07}="Sì",1,0)` | Sez. 14.7, 24, 36 |
| F08 | Autorizzazione sismica rilasciata prima dell'inizio dei lavori | elenco | SN |  | X |  | X | X |  | `IF({F07}="Acquisita",1,0)` | Art. 94 DPR 380/2001: sez. 14.7 |
| F09 | Agibilità: finiture ultimate | elenco | SN |  |  |  | X |  |  |  | Sez. 14.3 (art. 24) |
| F10 | Agibilità: collaudo statico | elenco | SNNN |  |  |  | X |  |  |  | Sez. 14.3 |
| F11 | Agibilità: certificati degli impianti | elenco | SN |  |  |  | X |  |  |  | Sez. 14.3 |
| F12 | Agibilità: requisiti di accessibilità | elenco | SNNN |  |  |  | X |  |  |  | Sez. 14.3 |
| F13 | Agibilità: aggiornamento catastale | elenco | SN |  |  |  | X |  |  |  | Sez. 14.8 |
| F14 | Agibilità: ricorso alle deroghe dell'art. 24 c. 5-bis (altezza, mq) | elenco | SN |  |  |  | X |  |  |  | Sez. 14.3, 14.8 |

### Condoni
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| H00 | Sezione H pertinente? (procedimento di condono) | elenco | SWITCH |  | X | X | X | X |  |  | Interruttore: «No» = sezione saltata |
| H01 | Volume dell'opera abusiva (soglia dei 450 mc per la perizia) | numero |  | mc |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 12.1, 30.4, 8 n. 16 |
| H02 | Ampliamento abusivo | numero |  | mc |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 12.1 |
| H03 | Ampliamento sulla volumetria originaria (calcolato) | calcolato | `IF(AND(ISNUMBER({E14}),ISNUMBER({H02}),{E14}>0),{H02}/{E14},"")` | % |  |  |  |  | `IF({H00}="Sì",1,0)` |  | Cella calcolata (usa la volumetria originaria del blocco E) |
| H04 | Nuova costruzione residenziale abusiva | numero |  | mc |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 12.1; 21.1 (zona rossa) |
| H05 | Opere conformi alle norme e agli strumenti urbanistici | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 12.2 (tipologia 1 o 2); 30.1 |
| H06 | Domanda presentata nei termini (L. 326/03: sul modello entro il 31/03/2004) | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 12.5, 30.3 |
| H07 | Oblazione e oneri versati | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 12.5, 21.1; oneri anni precedenti: segnalati, non calcolati (8 n. 27) |
| H08 | Documentazione fotografica | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 12.5 |
| H09 | Perizia giurata e idoneità statica allegate | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 30.4, 8 n. 16 (450 mc o sempre: segnalare) |
| H10 | Parere dell'amministrazione preposta al vincolo | elenco | PARERE |  |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 12.4, 12.9 |
| H11 | Possibilità di adeguamento antisismico | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 12.5 |
| H12 | Area demaniale o gravata da usi civici | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 12.3 |
| H13 | Area boscata o a pascolo percorsa dal fuoco negli ultimi 10 anni | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` |  | Sez. 12.3 |
| H14 | L. 326/03: tipologia di illecito (Allegato 1) | elenco | TIPOL |  |  | X |  |  | `IF({H00}="Sì",1,0)` | `IF({A02}="Condono L. 326/03",1,0)` | Sez. 30.1 |
| H15 | L. 326/03: lettera dell'art. 3 c. 1 DPR 380 (tipologie 3-6) | elenco | LETT380 |  |  | X |  |  | `IF({H00}="Sì",1,0)` | `IF(AND({A02}="Condono L. 326/03",{H14}<>"",IFERROR(VALUE(LEFT({H14},1)),0)>=3),1,0)` | Sez. 30.1 |
| H16 | L. 326/03: opere completate al rustico entro il 31/03/2003 | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` | `IF({A02}="Condono L. 326/03",1,0)` | Sez. 21.1 (art. 3 c. 2 lett. b LR 10/2004) |
| H17 | L. 326/03: destinazione residenziale delle opere (zona rossa) | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` | `IF({A02}="Condono L. 326/03",1,0)` | Sez. 21.1 (art. 3 c. 2 lett. d) |
| H18 | L. 326/03: versamento del 30% di oblazione e anticipazione oneri (minimi 1.700 € e 500 €) | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` | `IF({A02}="Condono L. 326/03",1,0)` | Sez. 30.3 |
| H19 | L. 326/03: rate del 30/06 e 30/09/2004 versate | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` | `IF({A02}="Condono L. 326/03",1,0)` | Sez. 30.3 |
| H20 | L. 326/03: oneri definitivi versati | elenco | ONERIDEF |  |  | X |  |  | `IF({H00}="Sì",1,0)` | `IF({A02}="Condono L. 326/03",1,0)` | Sez. 30.3 (termine 31/12/2006) |
| H21 | L. 326/03: dichiarazione ex art. 4 L. 15/1968 presentata | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` | `IF({A02}="Condono L. 326/03",1,0)` | Sez. 30.3 |
| H22 | L. 326/03: integrazione del 30/09/2004 (denuncia catastale, ICI, tassa rifiuti, occupazione suolo) | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` | `IF({A02}="Condono L. 326/03",1,0)` | Sez. 30.3 |
| H23 | L. 47/85 e L. 724/94: dichiarazione sostitutiva ex art. 9 c. 2 LR 10/2004 presentata | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` | `IF(OR({A02}="Condono L. 47/85",{A02}="Condono L. 724/94"),1,0)` | Sez. 21.2 (termine 31/12/2026) |
| H24 | L. 47/85 e L. 724/94: adeguamento sismico eseguito | elenco | SN |  |  | X |  |  | `IF({H00}="Sì",1,0)` | `IF(OR({A02}="Condono L. 47/85",{A02}="Condono L. 724/94"),1,0)` | Sez. 21.2 |

### Leggi regionali
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| I00 | Sezione I pertinente? (leggi regionali di settore) | elenco | SWITCH |  | X | X | X | X |  |  | Interruttore: «No» = sezione saltata |
| I01 | Tema regionale pertinente | elenco | LRTIPO |  | X | X | X | X | `IF({I00}="Sì",1,0)` |  | Sez. 16-22 |
| I02 | Edificio accatastato | elenco | SN |  | X | X |  | X | `IF({I00}="Sì",1,0)` |  | Sez. 18 (LR 13/2022) |
| I03 | Destinazione residenziale prevalente | elenco | SN |  | X | X |  | X | `IF({I00}="Sì",1,0)` |  | Sez. 18, 20 |
| I04 | Piscina interrata: superficie | numero |  | mq | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Piscina interrata",1,0)` | LR 13/2022 art. 2 (piscine fino a 24 mq): sez. 18 |
| I05 | Piscina interrata: profondità | numero |  | m | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Piscina interrata",1,0)` | Sez. 18 |
| I06 | Muro di cinta/recinzione: altezza | numero |  | m | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Muro di cinta / recinzione",1,0)` | Sez. 18 |
| I07 | Muro di cinta/recinzione: materiale | elenco | RECMAT |  | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Muro di cinta / recinzione",1,0)` | Sez. 18 |
| I08 | Sottotetto: data di esistenza | data |  |  | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Recupero abitativo del sottotetto",1,0)` | Sez. 16.3, 19, 8 n. 8 |
| I09 | Sottotetto: collegamento all'unità sottostante | elenco | SN |  | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Recupero abitativo del sottotetto",1,0)` | Sez. 19 |
| I10 | Sottotetto: ampliamento di unità preesistente (in zona rossa) | elenco | SN |  | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Recupero abitativo del sottotetto",1,0)` | Sez. 16.3, 17 |
| I11 | Sottotetto: frazionamento in nuova unità | elenco | SN |  | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Recupero abitativo del sottotetto",1,0)` | Sez. 16.3 |
| I12 | Sottotetto: legittimità o sanatoria dell'edificio | elenco | LEGIT |  | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Recupero abitativo del sottotetto",1,0)` | Sez. 19 |
| I13 | Sottotetto: area esclusa dal Consiglio comunale | elenco | SN |  | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Recupero abitativo del sottotetto",1,0)` | LR 15/2000 art. 1 c. 2: sez. 8 n. 8 |
| I14 | Sottotetto: altezza media | numero |  | m | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Recupero abitativo del sottotetto",1,0)` | Art. 43 quater LR 16/2004 (≥ 2,10 m): sez. 16.3 |
| I15 | Sottotetto: altezza al colmo | numero |  | m | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Recupero abitativo del sottotetto",1,0)` | Sez. 16.3 |
| I16 | Sottotetto: altezza ai muri perimetrali | numero |  | m | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Recupero abitativo del sottotetto",1,0)` | Sez. 16.3 |
| I17 | Sottotetto: altezza minima di parete (≥ 1,40 m) | numero |  | m | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Recupero abitativo del sottotetto",1,0)` | Sez. 16.3 |
| I18 | Delocalizzazione: unità adibita a prima casa | elenco | SN |  | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Delocalizzazione dalla zona rossa",1,0)` | Sez. 20.3 (art. 11 bis LR 19/2009) |
| I19 | Delocalizzazione: quota di volumetria originaria destinata a uso non residenziale | numero |  | % | X | X |  | X | `IF({I00}="Sì",1,0)` | `IF({I01}="Delocalizzazione dalla zona rossa",1,0)` | Sez. 17, 20.3 (almeno 50%) |

### Contributo di costruzione
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| K00 | Sezione K pertinente? (si calcola il contributo) | elenco | SWITCH |  | X | X | X | X |  |  | Interruttore: «No» = sezione saltata |
| K01 | Tipologia ai fini del K3 | elenco | K3TIPO |  | X | X |  | X | `IF({K00}="Sì",1,0)` |  | Sez. 29.3 |
| K02 | Classe di indice di fabbricabilità (If) | elenco | IFCLASS |  | X | X |  | X | `IF({K00}="Sì",1,0)` | `IF(LEFT({K01},12)="Residenziale",1,0)` | Sez. 29.3 |
| K03 | Volume «vuoto per pieno» (V) | numero |  | mc | X | X |  | X | `IF({K00}="Sì",1,0)` |  | Sez. 29.2 (≠ V.C. dell'Allegato 1 NTA: sez. 8 n. 15 d) |
| K04 | Superficie utile (Su) per il costo di costruzione | numero |  | mq | X | X |  | X | `IF({K00}="Sì",1,0)` |  | Sez. 29.5, 29.8 |
| K05 | Superficie non residenziale (Snr) | numero |  | mq | X | X |  | X | `IF({K00}="Sì",1,0)` |  | Sez. 29.5, 29.8 |
| K06 | Importo del computo metrico dei lavori (per l'esistente) | numero |  | € | X | X |  | X | `IF({K00}="Sì",1,0)` |  | Sez. 29.8 (prassi d'ufficio: 10%, computo metrico) |
| K07 | Impianti produttivi: numero di addetti | numero |  | n. | X | X |  | X | `IF({K00}="Sì",1,0)` | `IF(LEFT({K01},8)="Impianti",1,0)` | Sez. 29.4 (Up1) |
| K08 | Impianti produttivi: superficie S1 | numero |  | mq | X | X |  | X | `IF({K00}="Sì",1,0)` | `IF(LEFT({K01},8)="Impianti",1,0)` | Sez. 29.4 |
| K09 | Impianti produttivi: superficie S2 | numero |  | mq | X | X |  | X | `IF({K00}="Sì",1,0)` | `IF(LEFT({K01},8)="Impianti",1,0)` | Sez. 29.4 |
| K10 | Impianti produttivi: superficie S3 | numero |  | mq | X | X |  | X | `IF({K00}="Sì",1,0)` | `IF(LEFT({K01},8)="Impianti",1,0)` | Sez. 29.4 |
| K11 | Impianti produttivi: classe dell'industria (Cj) | testo |  |  | X | X |  | X | `IF({K00}="Sì",1,0)` | `IF(LEFT({K01},8)="Impianti",1,0)` | Sez. 29.4 |

### Relazione paesaggistica
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| L00 | Sezione L pertinente? (serve l'autorizzazione paesaggistica) | elenco | SWITCH |  | X | X | X | X |  |  | Interruttore: «No» = sezione saltata |
| L01 | Procedimento paesaggistico | elenco | PROCPAES |  | X | X |  | X | `IF({L00}="Sì",1,0)` |  | Sez. 13.4, 13.6, 31; 8 n. 28 (vale la relazione semplificata del DPR 31) |
| L02 | Relazione paesaggistica allegata | elenco | SN |  | X | X |  | X | `IF({L00}="Sì",1,0)` |  | Sez. 31 |
| L03 | Contenuto: analisi dello stato attuale | elenco | SN |  | X | X |  | X | `IF({L00}="Sì",1,0)` |  | Sez. 31 (DPCM 12/12/2005) |
| L04 | Contenuto: livelli di tutela e beni culturali | elenco | SN |  | X | X |  | X | `IF({L00}="Sì",1,0)` |  | Sez. 31 |
| L05 | Contenuto: foto da 2 a 4 dal contesto e dai punti panoramici | elenco | SN |  | X | X |  | X | `IF({L00}="Sì",1,0)` |  | Sez. 31 |
| L06 | Contenuto: elaborati di progetto con prospetti e colori | elenco | SN |  | X | X |  | X | `IF({L00}="Sì",1,0)` |  | Sez. 31 |
| L07 | Contenuto: simulazione (rendering o fotoinserimento) | elenco | SN |  | X | X |  | X | `IF({L00}="Sì",1,0)` |  | Sez. 31 |
| L08 | Contenuto: effetti, mitigazioni e compensazioni | elenco | SN |  | X | X |  | X | `IF({L00}="Sì",1,0)` |  | Sez. 31 |
| L09 | Edificio esistente: rilievo dello stato di fatto e documentazione storica | elenco | SN |  | X | X |  | X | `IF({L00}="Sì",1,0)` |  | Sez. 31 |

### Edilizia libera
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| M00 | Sezione M pertinente? (edilizia libera o manufatti temporanei) | elenco | SWITCH |  | X | X | X | X |  |  | Interruttore: «No» = sezione saltata |
| M01 | Numero d'opera del glossario (1-58) | numero |  | n. | X |  |  |  | `IF({M00}="Sì",1,0)` |  | Sez. 33.3 |
| M02 | Superficie coperta del manufatto (limite 30 mq per le «limitate dimensioni») | numero |  | mq | X |  |  |  | `IF({M00}="Sì",1,0)` |  | Sez. 33; 8 n. 28 (30 mq = superficie coperta, solo pertinenze ammesse dalle NTA) |
| M03 | Altezza del manufatto | numero |  | m | X |  |  |  | `IF({M00}="Sì",1,0)` |  | Sez. 33.3 |
| M04 | Tipo di ancoraggio | elenco | ANCORAGGIO |  | X |  |  |  | `IF({M00}="Sì",1,0)` |  | Sez. 33.3 |
| M05 | Durata prevista (opere temporanee: max 180 giorni, non reiterabile) | numero |  | giorni | X |  |  |  | `IF({M00}="Sì",1,0)` |  | Art. 6 lett. e-bis: sez. 8 n. 18 e 28 |
| M06 | Uso del manufatto | elenco | MUSO |  | X |  |  |  | `IF({M00}="Sì",1,0)` |  | Sez. 33.3 |
| M07 | Interno al centro edificato | elenco | SN |  | X |  |  |  | `IF({M00}="Sì",1,0)` |  | Sez. 33.3 |
| M08 | Indice di permeabilità dell'area | numero |  | % | X |  |  |  | `IF({M00}="Sì",1,0)` |  | Sez. 33.3 |

### Piano di protezione civile
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| N00 | Sezione N pertinente? (strade-cancello, aree di attesa, aumento residenti) | elenco | SWITCH |  | X | X | X | X |  |  | Interruttore: «No» = sezione saltata |
| N01 | Edificio/area coincidente con area di attesa, area di ricovero o sede COC | elenco | SN |  | X |  |  | X | `IF({N00}="Sì",1,0)` |  | Sez. 34.3, 34.7 |
| N02 | Intervento che incide su carreggiata, sedime o accesso di una strada-cancello | elenco | SN |  | X |  |  | X | `IF({N00}="Sì",1,0)` |  | Sez. 34.3 (attenzione, non diniego: 8 n. 28) |
| N03 | Quale strada-cancello (indicare la denominazione dal piano) | testo |  |  | X |  |  | X | `IF({N00}="Sì",1,0)` | `IF({N02}="Sì",1,0)` | Sez. 34.7 |
| N04 | Aumento dei residenti previsti (unità abitative, posti letto, strutture ricettive o di assistenza) | elenco | SN |  | X |  |  | X | `IF({N00}="Sì",1,0)` |  | Sez. 34.7 |
| N05 | Entità dell'aumento | numero |  | n. | X |  |  | X | `IF({N00}="Sì",1,0)` | `IF({N04}="Sì",1,0)` | Sez. 34.7 |

### Competenza sismica
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| S00 | Sezione S pertinente? (lavori su parti strutturali) | elenco | SWITCH |  | X | X | X | X |  |  | Interruttore: «No» = sezione saltata |
| S01 | Altezza strutturale (dal piano di posa delle fondazioni all'estradosso del torrino scala o al colmo) | numero |  | m | X | X |  | X | `IF({S00}="Sì",1,0)` |  | Sez. 36 (limite 16,50 m) |
| S02 | Profondità della parte interrata | numero |  | m | X | X |  | X | `IF({S00}="Sì",1,0)` |  | Sez. 36 (limite 4,00 m) |
| S03 | Istanza sismica già inoltrata al Genio Civile prima del 01/11/2025 | elenco | SN |  | X | X |  | X | `IF({S00}="Sì",1,0)` |  | Sez. 36 (LR 26/2025 art. 1 c. 1 ter) |

### Immobile con istanza di condono pendente
| Cod. | Campo | Tipo | Lista/Valore | Unità | ORD | CON | AGI | ACC | Interr. | Condizione | Nota |
|---|---|---|---|---|---|---|---|---|---|---|---|
| P00 | Sezione P pertinente? (condono L. 47/85 o L. 724/94 pendente sull'immobile) | elenco | SWITCH |  | X | X | X | X |  |  | Interruttore: «No» = sezione saltata |
| P01 | Legge dell'istanza di condono pendente | elenco | LEGGEPEND |  | X | X | X | X | `IF({P00}="Sì",1,0)` |  | Sez. 37.3 (non vale per la L. 326/2003) |
| P02 | Tipo di intervento richiesto | elenco | PTIPO |  | X | X | X | X | `IF({P00}="Sì",1,0)` |  | Sez. 37.2 |
| P03 | Cambio d'uso, frazionamento o accorpamento di unità | elenco | SN |  | X | X | X | X | `IF({P00}="Sì",1,0)` |  | Sez. 37.2 (condizioni ii) |
| P04 | Modifiche ai prospetti, alla superficie o al volume | elenco | SN |  | X | X | X | X | `IF({P00}="Sì",1,0)` |  | Sez. 37.2 (condizioni iii e iv) |
| P05 | Fotovoltaico/termico in copertura visibile da aree pubbliche o punti panoramici | elenco | SN |  | X | X | X | X | `IF({P00}="Sì",1,0)` |  | Sez. 37.2 n. 3 |
| P06 | Valutazione della sicurezza NTC (§ 8.3) acquisita | elenco | SN |  | X | X | X | X | `IF({P00}="Sì",1,0)` |  | Sez. 37.2 n. 2 |
| P07 | Autodichiarazione e perizia giurata presenti | elenco | SN |  | X | X | X | X | `IF({P00}="Sì",1,0)` |  | Sez. 37.2 n. 2-3 |
| P08 | Oblazione e oneri versati integralmente | elenco | SN |  | X | X | X | X | `IF({P00}="Sì",1,0)` |  | Sez. 37.3 (oneri: segnalati, non calcolati) |
| P09 | Ordinanze, sequestri o ricorsi pendenti | elenco | SN |  | X | X | X | X | `IF({P00}="Sì",1,0)` |  | Sez. 37.3 |
| P10 | Opere eseguite dopo la presentazione della domanda di condono | elenco | SN |  | X | X | X | X | `IF({P00}="Sì",1,0)` |  | Sez. 37.2 n. 3 |

Legenda colonne: ORD/CON/AGI/ACC = regola di obbligatorietà per gruppo (`X` sempre; `P`/`C`/`DR`/`T` solo se la qualificazione ha quel flag; vuoto = non richiesto). Interr. = condizione sull'interruttore di sezione. «Lista» rimanda ai nomi di §6.

## 6. Elenchi (valori ammessi)

### Tipo di procedimento → Gruppo
| Procedimento | Gruppo |
|---|---|
| PdC | ORD |
| SCIA | ORD |
| SCIA alternativa al PdC | ORD |
| CILA | ORD |
| Edilizia libera | ORD |
| Condono L. 47/85 | CON |
| Condono L. 724/94 | CON |
| Condono L. 326/03 | CON |
| Agibilità | AGI |
| Accertamento di conformità art. 36 | ACC |
| Accertamento di conformità art. 36-bis | ACC |

### Qualificazione dell'intervento → flag
| Qualificazione | P | C | DR | T |
|---|---|---|---|---|
| Manutenzione ordinaria | 0 | 0 | 0 | 0 |
| Manutenzione straordinaria | 0 | 0 | 0 | 0 |
| Restauro e risanamento conservativo | 0 | 0 | 0 | 0 |
| Ristrutturazione edilizia senza demolizione | 1 | 1 | 0 | 0 |
| Ristrutturazione con demolizione e ricostruzione | 1 | 1 | 1 | 0 |
| Ristrutturazione urbanistica | 1 | 1 | 0 | 0 |
| Nuova costruzione | 1 | 1 | 0 | 0 |
| Ampliamento | 1 | 1 | 0 | 0 |
| Sopraelevazione | 1 | 1 | 0 | 0 |
| Cambio d'uso senza opere | 0 | 1 | 0 | 0 |
| Cambio d'uso con opere | 1 | 1 | 0 | 0 |
| Intervento «tetto» (copertura piana → tetto) | 1 | 0 | 0 | 1 |
| Recupero abitativo del sottotetto | 1 | 1 | 0 | 0 |
| Pertinenze e manufatti (tettoie, autorimesse, volumi tecnici) | 1 | 0 | 0 | 0 |
| Demolizione senza ricostruzione | 0 | 0 | 0 | 0 |
| Altro (specificare nella descrizione) | 1 | 1 | 0 | 0 |

### Ambito PUC → articolo NTA → zona A-F di default → colonna K3 → agricolo
| Ambito | Art. | Zona default | Colonna K3 | Agricolo |
|---|---|---|---|---|
| Art. 7 – Collinari e montani di tutela naturale | 7 | E | E | 1 |
| Art. 8 – Agricoli di salvaguardia | 8 | E | E | 1 |
| Art. 9 – Agricoli pedecollinari e di pianura | 9 | E | E | 1 |
| Art. 10 – Agricoli periurbani | 10 | E | E | 1 |
| Art. 11 – Agricoli urbani | 11 | E | E | 1 |
| Art. 12 – Florovivaismo | 12 | E | E | 1 |
| Art. 14 – Recupero e valorizzazione (centri storici) | 14 | A | A | 0 |
| Art. 15 – Urbani consolidati | 15 | B | B – intervento diretto | 0 |
| Art. 16 – Urbani di riassetto, parti edificate | 16 | B | B – intervento diretto | 0 |
| Art. 16 – Urbani di riassetto, parti libere soggette a PUA | 16 | B-PUA | B – PUA (foglio ufficio) | 0 |
| Art. 17 – Ambiti di trasformabilità (disposizioni generali) | 17 | C | non in tabella 35.0: indicare la zona | 0 |
| Art. 18 – Trasformabilità strategica ATS-1 (decaduto), ATS-3, 4, 5 | 18 | C | C | 0 |
| Art. 19 – Trasformabilità integrata (ATI) | 19 | D | C | 0 |
| Art. 20 – Produttive esistenti interne | 20 | D | D | 0 |
| Art. 21 – Produttive esistenti esterne | 21 | D | D | 0 |
| Art. 22 – Produttive di riassetto e completamento | 22 | D | D | 0 |
| Art. 23 – Produttive incompatibili | 23 | D | D | 0 |
| Art. 24 – Produttive dismesse | 24 | D | D | 0 |
| Art. 25 – Distributori di carburante | 25 | B | D | 0 |
| Art. 26 – Attività turistiche e ricettive esistenti | 26 | D | B – intervento diretto | 0 |
| Art. 27 – Trasformabilità per attività produttive (ATP) | 27 | F | D | 0 |
| Art. 28 – Attrezzature pubbliche locali | 28 | F | F | 0 |
| Artt. 29 e 47 – Attrezzature pubbliche territoriali | 29 | F | F | 0 |
| Art. 30 – Attrezzature private esistenti | 30 | F | F | 0 |
| Art. 30 bis – Attrezzature private dismesse | 30 bis | F | F | 0 |

### Liste semplici
- **SN**: Sì · No · Non noto
- **SNNN**: Sì · No · Non necessario · Non noto
- **ACQ**: Acquisita · Non acquisita · Non necessaria · Non noto
- **ZONA**: A · B · B-PUA · C · D · E · F
- **RISCHIO**: Nessuno · R1 · R2 · R3 · R4 · Non noto
- **INAREA**: Interamente · Parzialmente · No · Non noto
- **PTP**: PI · RUA · Altra zona PTP · Fuori perimetro PTP · Non noto
- **PARCO**: Dentro · Contiguo · Fuori · Non noto
- **FASCIA**: Nessuna · Stradale · Ferroviaria · Fluviale o canali · Elettrodotto · Captazione acque · Cimiteriale · Più di una · Non noto
- **DPR31**: Nessuna voce · Allegato A (esclusa) · Allegato B (semplificata) · Non noto
- **CATSISM**: Rilevante · Di minore rilevanza · Privo di rilevanza
- **TITORIG**: Permesso di costruire / licenza / concessione · SCIA / DIA · Condono rilasciato · Nessun titolo (abusivo) · Nessun titolo (preesistente all'obbligo di titolo) · Non noto
- **EPOCA**: Ante 1945 · 1945-1955 · Dopo il 1955 · Non nota
- **STATOLEG**: Accertato · Da acquisire d'ufficio
- **DIFFQUAL**: Assenza di titolo · Totale difformità · Variazioni essenziali · Parziale difformità · Tolleranza (art. 34-bis)
- **DOCDATA**: Foto aeree / ortofoto · Catasto storico · Atto o certificazione ufficiale · Altra documentazione · Nessuna
- **PARKUBIC**: Nel lotto (in superficie) · Interrato · Fuori lotto · Non previsti
- **USO**: A – Abitativo · B – Industriale/artigianale · C – Terziario · D – Commerciale · E – Ricettivo · F – Ricreativo · G – Attrezzature pubbliche · H – Rurale
- **PARERE**: Acquisito · Non acquisito · Dissenso · Non richiesto
- **TIPOL**: 1 – Senza titolo, non conformi · 2 – Senza titolo, conformi · 3 – Ristrutturazione edilizia · 4 – Restauro e risanamento (zona A) · 5 – Restauro e risanamento · 6 – Manutenzione straordinaria
- **LETT380**: b · c · d
- **ONERIDEF**: Sì, entro il termine · Sì, oltre il termine · No
- **LRTIPO**: Recupero abitativo del sottotetto · Piscina interrata · Muro di cinta / recinzione · Delocalizzazione dalla zona rossa · Altro
- **RECMAT**: Muratura · Calcestruzzo · Rete metallica / siepe · Altro
- **LEGIT**: Legittimo · Sanato L. 47/85 · Sanato L. 724/94 · Sanato L. 326/03 · Non legittimo
- **K3TIPO**: Residenziale (nuova costruzione) · Edilizia convenzionata o sovvenzionata · Terziaria direzionale o commerciale · Turistica residenziale · Attrezzature di tempo libero · Ristrutturazione con incremento di superficie · Ristrutturazione con variazione d'uso · Ristrutturazione non convenzionata fuori legge · Ristrutturazione urbanistica (foglio ufficio) · Impianti industriali e artigianali · Altro (descrivere)
- **IFCLASS**: If < 1,50 mc/mq · 1,50 ≤ If ≤ 3,00 mc/mq · If > 3,00 mc/mq
- **PROCPAES**: Autorizzazione ordinaria (art. 146) · Semplificata (DPR 31/2017) · Scheda semplificata (DPCM 12/12/2005) · Non necessaria · Non noto
- **ANCORAGGIO**: Ancorato stabilmente · Amovibile · Non noto
- **MUSO**: Agricolo · Pertinenziale · Altro
- **LEGGEPEND**: L. 47/85 · L. 724/94
- **PTIPO**: Manutenzione ordinaria · Manutenzione straordinaria · Efficientamento energetico · Restauro e risanamento conservativo · Altro (eccedente)
- **SWITCH**: Sì · No

## 7. Controlli (13 segnalazioni automatiche, non sostituiscono il parere)

1. Zona A-F applicata (B04) e fonte (scheda o default d'ambito).
2. Vincolo paesaggistico anteriore/posteriore alle opere (condoni e accertamenti: date B14-B16 vs data opere).
3. Condono senza sezione H compilata.
4. Edilizia libera senza sezione M.
5. Sottotetto senza sezione I.
6. Lavori strutturali senza sezione S.
7. Competenza sismica: Comune o Genio Civile (altezza strutturale > 16,50 m, interrato > 4,00 m, istanza prima del 01/11/2025 → Genio Civile; LR 26/2025).
8. Rischio PSAI R3/R4 (solo rischio; la pericolosità non si considera).
9. Rischio atteso PSAI: segnalare senza concludere.
10. Delibera C.C. 22/2023 (immobile con condono pendente: entro/oltre i limiti).
11. Date incoerenti (es. fine lavori < inizio, titolo < opere).
12. Zona rossa vesuviana (LR 21/2003): volumi abitabili o uso abitativo → segnalare.
13. Zona non determinata per il K3.

Il foglio «Controlli» contiene inoltre, per ogni campo, la tabella: regola ORD/CON/AGI/ACC, pertinenza, condizione, **stato** (`ok`, `MANCANTE`, `NON NOTO`) e il riepilogo (richiesti/compilati/mancanti/non noti).

## 8. Esempi di collaudo

- SCIA di cambio d'uso senza opere → **41** campi richiesti.
- CILA di manutenzione straordinaria su immobile con condono pendente (blocco P) → **44** campi richiesti.
File: `scheda/scheda-anonima-esempio-fittizio.xlsx` (due casi fittizi) da usare come test dell'app: l'app deve produrre gli stessi conteggi.

## 9. Uscita attesa dall'app → Project Claude → PDF

Scheda esportata (Markdown/JSON) → agente applica sequenza di verifica (sintesi sez. 2) → parere (formato sez. 11 della sintesi) → **PDF** con stemma, grafica del Comune, esito positivo/negativo, motivazione, riferimenti normativi precisi, mancanti, segnalazioni, alternative, dicitura sulla firma del funzionario.

## 10. Scelte di progetto da confermare

1. B14-B16 (date vincoli): obbligatorie solo per condoni e accertamenti.
2. Collaudo pratica 2 letta come CILA su immobile con istanza pendente (blocco P).
3. Campo aggiunto D22 (piano di sviluppo aziendale).
4. Flag: ristrutturazione senza D&R = P e C; manutenzione straordinaria/restauro = né P né C; cambio d'uso senza opere = solo C.
5. Stato di fatto → progetto: due celle per S.C., S.U., V.C., H, N.P., unità immobiliari.
6. Blocco H solo CON; L, K, S per ORD, ACC, CON; N solo ORD e ACC; P tutti.
7. Ambiti senza default (art. 17, 32-35, Altro): zona indicata dal funzionario.
8. «B-PUA» come valore di zona (assente nella delibera n. 17/2025 per il K3).
9. Classi d'uso RUEC A-H.
