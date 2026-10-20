# Scheda anonima – elenco dei campi (v1, 20/10/2026)

File: `scheda/scheda-anonima-v1.xlsx` (fogli Istruzioni, Scheda, Controlli, Elenchi). Esempio di collaudo con dati fittizi: `scheda/scheda-anonima-esempio-fittizio.xlsx`. Questo file serve alle **istruzioni del Project** (CLAUDE.md sez. 5 punto 4): mette in corrispondenza **codice campo → etichetta → sezione della sintesi → procedimenti per cui è obbligatorio**, e dice quali voci della sez. 10 della sintesi **non** sono nella scheda e perché. La sez. 10 della sintesi resta l'elenco completo (non è stata modificata).

## 1. Come leggere la scheda (per il Project)

- **Una riga per campo**, colonne stabili: `Codice | Sezione | Campo | Valore | Unità | Obbligatorio per | Nota | Stato`. Il valore è nella colonna D del foglio «Scheda». I codici (A01, B05, E14 …) e le etichette non cambiano tra le versioni 1.x; se un campo viene aggiunto riceve un nuovo codice in coda al blocco. I codici dei campi calcolati (es. B02) restano riservati.
- **Menu a tendina** per i campi a scelta chiusa (foglio «Elenchi»); valori «Non noto» ammessi. **Vuoto** = dato non fornito. Nel parere i dati vuoti o «Non noto» richiesti vanno elencati come mancanti (CLAUDE.md sez. 2 punto 3), mai supposti.
- **Gruppi di procedimento** (foglio Elenchi): `ORD` = PdC, SCIA, SCIA alternativa al PdC, CILA, edilizia libera; `CON` = condono L. 47/85, L. 724/94, L. 326/03; `AGI` = agibilità; `ACC` = accertamento di conformità (art. 36 e 36-bis).
- **Flag della qualificazione** (foglio Elenchi, tabella delle qualificazioni): `P` = l'intervento ha parametri edilizi (volumi, sagoma, superfici); `C` = può cambiare uso o carico; `DR` = demolizione e ricostruzione; `T` = intervento «tetto». Un campo con regola `ORD:P` è richiesto per i titoli ordinari solo se la qualificazione scelta ha il flag P.
- **Interruttori** «Sezione … pertinente?» (Sì/No) per i blocchi H, I, K, L, M, N, S, P: con «No» i campi del blocco non sono richiesti (righe grigie). Nessun interruttore è precompilato: ognuno è obbligatorio.
- **Dati noti per Ottaviano** (blocco Z, costanti precompilate): zona sismica 2; zona rossa; vincolo paesaggistico su tutto il territorio; PSAI 2015 (solo rischio); commissione sismica comunale attiva; delibera oneri n. 17/2025 con K2 = 0,59; NTA prevalenti sul RUEC.
- **Zona A-F**: B02 = default calcolato dall'ambito (sez. 35.0, assimilazione e non norma); B03 = zona indicata dal funzionario, che **prevale**; B04 = zona applicata. Per art. 17 e artt. 32-35 e «Altro» non c'è default.

**Conteggi**: la sez. 10 conta **180 voci elementari** (39 voci numerate A-F con i loro sotto-dati + blocchi G-O). La scheda ha **221 righe**: **202 campi da compilare**, 8 interruttori, 7 costanti, 4 celle calcolate. Perché il numero di campi non scende: i dati composti della sez. 10 (es. «S.F., S.C., S.U., V.C., H, N.P.», «stato di fatto → progetto», le quattro grandezze delle tolleranze, assentite e realizzate) sono scomposti in celle singole, necessarie per essere lette in modo univoco. La riduzione è **per pratica**: i campi richiesti dipendono da procedimento, qualificazione e interruttori (nel collaudo: **41** campi richiesti per una SCIA di cambio d'uso senza opere, **44** per una CILA su immobile con condono pendente). **25 voci** della sez. 10 sono escluse (sez. 3 di questo file); **piano_az** è l'unico campo aggiunto.

## 2. Campi della scheda

### Z – Dati noti per Ottaviano (precompilati, non modificare)

*Costanti del Comune: non vanno compilate.*

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| Z01 | Zona sismica | costante | — | Sez. 14.7, 24 (conferma dell'utente) |
| Z02 | Zona rossa vesuviana (LR 21/2003) | costante | — | Sez. 17; la LR 21/2003 resta in vigore (PPR non in vigore: sez. 8 n. 28) |
| Z03 | Vincolo paesaggistico | costante | — | Sez. 13, 37.1 |
| Z04 | PSAI vigente | costante | — | Sez. 5, 5 bis; CLAUDE.md sez. 4 |
| Z05 | Funzioni sismiche | costante | — | Sez. 36; sez. 8 n. 28 |
| Z06 | Delibera oneri e K2 | costante | — | Sez. 29, 8 n. 22 e 27 |
| Z07 | Prevalenza NTA / RUEC | costante | — | CLAUDE.md sez. 2 punto 4; sez. 8 n. 1 e 27 |

### A – Procedimento

*Sempre da compilare.*

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| A01 | Codice pratica anonimo (scelto dall'ufficio, senza nomi né numeri di protocollo identificativi) | testo | Tutti | Sez. 10 A |
| A02 | Tipo di procedimento | elenco PROC | Tutti | Sez. 10 A1; 7, 12, 14, 32 |
| A03 | Qualificazione dell'intervento (DPR 380/2001) | elenco QUAL | Ordinari; Accertamento | Sez. 10 A2; 14.1, 28.3, 32. Per i condoni vale la tipologia di illecito (sez. H) |
| A04 | Descrizione sintetica delle opere (solo dati tecnici, nessun dato personale) | testo | Ordinari; Condoni; Accertamento | Sez. 10 A3 |
| A05 | Data di presentazione della domanda / del titolo | data | Tutti | Sez. 10 A4; 14.2; per la SCIA/CILA decorrenza della delibera oneri (29.1) |
| A06 | Data di rilascio del PdC (solo PdC; decorrenza della delibera oneri) | data | Ordinari – solo se PdC | Sez. 10 A4; 29.1 |

### B – Localizzazione urbanistica e vincoli

*Tutto il territorio è vincolato paesaggisticamente (costante Z03): qui solo ciò che varia da pratica a pratica.*

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| B01 | Ambito PUC (articolo NTA) | elenco AMBITO | Ordinari; Condoni; Accertamento | Sez. 3, 28, 35.0 |
| B02 | Zona omogenea A-F di default (calcolata dall'ambito) | calcolato | — | Sez. 35.0 (assimilazione, non norma). Cella calcolata: non compilare |
| B03 | Zona omogenea A-F, se nota (prevale sul default dell'ambito) | elenco ZONA | Facoltativo | Sez. 35.0 e 35.6: la zona della scheda prevale |
| B04 | Zona omogenea applicata (calcolata) | calcolato | — | Cella calcolata: zona della scheda, altrimenti default dell'ambito. Se vuota e serve il K3, il parere la elenca come dato mancante |
| B05 | Zona PTP | elenco PTP | Ordinari; Condoni; Accertamento | Sez. 4; NTA art. 3 c. 2-3 |
| B06 | Rapporto con il Parco Nazionale del Vesuvio | elenco PARCO | Ordinari; Condoni; Accertamento | Sez. 4, 32.2 (nulla osta Ente Parco) |
| B07 | Interessa SIC/ZPS | elenco SN | Ordinari; Condoni; Accertamento | Sez. 4 |
| B08 | Area boscata | elenco SN | Ordinari; Condoni; Accertamento | Sez. 4 |
| B09 | Area archeologica | elenco SN | Ordinari; Condoni; Accertamento | Sez. 4 |
| B10 | Immobile bene culturale (art. 10 D.Lgs 42/04) o soggetto a tutela indiretta (artt. 45-47) | elenco SN | Ordinari; Condoni; Accertamento | Sez. 10 B7 e B10; 13.5, 13.8 |
| B11 | Fascia di rispetto interessata | elenco FASCIA | Ordinari [parametri]; Condoni; Accertamento [parametri] | Sez. 4; 8 n. 2 (fasce stradali). Se più di una, indicarle nella descrizione |
| B12 | Distanza dalla fascia / dall'elemento vincolante | numero (m) | Ordinari [parametri]; Accertamento [parametri] – solo se c'è una fascia | Sez. 10 B8 |
| B13 | Vincolo paesaggistico rafforzato (provvedimento specifico oltre al vincolo generale) | elenco SN | Ordinari; Condoni; Accertamento | Sez. 13.8 |
| B14 | Data di istituzione del vincolo paesaggistico applicabile all'immobile | data | Condoni; Accertamento | OBBLIGATORIO: CLAUDE.md sez. 4; sez. 12.3, 12.9. Va confrontata con la data delle opere (Controlli) |
| B15 | Data di istituzione del vincolo del Parco (o «non applicabile» se fuori Parco) | testo | Condoni; Accertamento | OBBLIGATORIO: sez. 10 B11 |
| B16 | Data di istituzione di altri vincoli presenti (idrogeologico, archeologico, ecc.; o «non applicabile») | testo | Condoni; Accertamento | OBBLIGATORIO: sez. 10 B11 |
| B17 | Allegato A o B del DPR 31/2017 applicabile | elenco DPR31 | Ordinari; Accertamento | Sez. 13.6, 13.8 |
| B18 | Voce dell'Allegato (es. B.16) | testo | Ordinari; Accertamento – solo se Allegato A o B | Sez. 13.6 |

### C – Rischi (PSAI, vulcanico, sismico)

*Per il PSAI si indica solo la classe di RISCHIO (la pericolosità non si considera).*

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| C01 | Rischio idraulico (PSAI) | elenco RISCHIO | Ordinari; Condoni; Accertamento | Sez. 5, 5 bis; 10 C13 |
| C02 | Rischio frana (PSAI) | elenco RISCHIO | Ordinari; Condoni; Accertamento | Sez. 5; 10 C14 (R3/R4 anche per art. 33 quater LR 16/2004) |
| C03 | Edificio in area a rischio | elenco INAREA | Ordinari; Condoni; Accertamento – solo se c'è un rischio | Sez. 10 C15 |
| C04 | Presenza di cavità o sinkholes | elenco SN | Ordinari [parametri]; Accertamento [parametri] | Sez. 10 C16 |
| C05 | Distanza da alvei/argini | numero (m) | Ordinari [parametri]; Accertamento [parametri] | Sez. 10 C16 |
| C06 | Rischio vulcanico/sismico segnalato dalle carte del PUC (P4.1, P4.3) | elenco SN | Ordinari [parametri]; Accertamento [parametri] | Sez. 10 C17; NTA art. 16 c. 2 |
| C07 | Lavori su parti strutturali | elenco SN | Ordinari; Accertamento | Sez. 14.7, 24 |
| C08 | Categoria dell'intervento ai fini sismici | elenco CATSISM | Ordinari; Accertamento – solo se lavori strutturali | Sez. 24 |

### D – Stato legittimo e difformità

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| D01 | Titolo originario dell'edificio | elenco TITORIG | Ordinari; Agibilità; Accertamento | Sez. 10 D19; 14.8 |
| D02 | Data del titolo originario (nessun numero di protocollo) | data | Ordinari; Agibilità; Accertamento – solo se esiste un titolo | Sez. 10 D19 |
| D03 | Epoca di costruzione | elenco EPOCA | Ordinari; Condoni; Accertamento | Sez. 3 (art. 15 c. 8: ante 1945/1955), 14.8 |
| D04 | Edificio già oggetto di condono rilasciato | elenco SN | Ordinari; Agibilità; Accertamento | Sez. 10 D19; 12 |
| D05 | Stato legittimo | elenco STATOLEG | Ordinari; Accertamento | Sez. 14.8, 26 (LR 31/2021 art. 27 c. 4: «da acquisire d'ufficio») |
| D06 | Difformità presenti rispetto allo stato legittimo | elenco SN | Tutti | Sez. 10 D20; 14.4 |
| D07 | Qualificazione proposta della difformità | elenco DIFFQUAL | Tutti – solo se ci sono difformità | Sez. 14.4 |
| D08 | Data di realizzazione dell'abuso / delle opere difformi | data | Condoni; Accertamento | Sez. 10 D21; 12.1, 14.5 |
| D09 | Data di ultimazione delle opere | data | Condoni; Agibilità; Accertamento | Sez. 10 D21; 12.1, 14.3 |
| D10 | Documentazione che prova la data | elenco DOCDATA | Condoni; Accertamento | Sez. 14.8; 12.5 |
| D11 | Superficie utile assentita dell'unità | numero (mq) | Accertamento – solo se ci sono difformità | Tolleranze art. 34-bis: sez. 14.5 |
| D12 | Volume assentito | numero (mc) | Accertamento – solo se ci sono difformità | Sez. 14.5, 14.8 |
| D13 | Volume realizzato | numero (mc) | Accertamento – solo se ci sono difformità | Sez. 14.5, 14.8 |
| D14 | Superficie coperta assentita | numero (mq) | Accertamento – solo se ci sono difformità | Sez. 14.5 |
| D15 | Superficie coperta realizzata | numero (mq) | Accertamento – solo se ci sono difformità | Sez. 14.5 |
| D16 | Altezza assentita | numero (m) | Accertamento – solo se ci sono difformità | Sez. 14.5 |
| D17 | Altezza realizzata | numero (m) | Accertamento – solo se ci sono difformità | Sez. 14.5 |
| D18 | Distacco minimo assentito | numero (m) | Accertamento – solo se ci sono difformità | Sez. 14.5 |
| D19 | Distacco minimo realizzato | numero (m) | Accertamento – solo se ci sono difformità | Sez. 14.5 |
| D20 | Titoli o domande sulla stessa unità negli ultimi 5 anni (elenco: tipo e anno, senza estremi) | testo | Ordinari [parametri] | Allegato 1 NTA art. 8 c. 2: sez. 28 |
| D21 | Uso agricolo del fondo documentato | elenco SN | Ordinari; Condoni; Accertamento – solo in ambiti agricoli (artt. 7-12) | NTA artt. 7-11: sez. 3 |
| D22 | Piano di sviluppo aziendale presentato (campo aggiunto) | elenco SN | Ordinari – solo nuova costruzione in ambito agricolo | Campo aggiunto rispetto alla sez. 10: NTA artt. 8, 9, 11 (sez. 3) |

### E – Parametri edilizi (stato di fatto → progetto)

*Compilare solo per gli interventi che cambiano volumi, superfici, usi o carico (vedi colonna «Obbligatorio per»).*

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| E01 | Superficie fondiaria (S.F.) | numero (mq) | Ordinari [parametri]; Accertamento [parametri] | Allegato 1 NTA: sez. 28 |
| E02 | Superficie coperta (S.C.) – stato di fatto | numero (mq) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28 |
| E03 | Superficie coperta (S.C.) – progetto | numero (mq) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28 |
| E04 | Superficie utile (S.U.) – stato di fatto | numero (mq) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28 |
| E05 | Superficie utile (S.U.) – progetto | numero (mq) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28 |
| E06 | Volume (V.C.) – stato di fatto | numero (mc) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28 |
| E07 | Volume (V.C.) – progetto | numero (mc) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28 |
| E08 | Altezza (H.F./H.M.) – stato di fatto | numero (m) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28 |
| E09 | Altezza (H.F./H.M.) – progetto | numero (m) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28 |
| E10 | Numero piani (N.P.) – stato di fatto | numero (n.) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28 |
| E11 | Numero piani (N.P.) – progetto | numero (n.) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28 |
| E12 | Unità immobiliari – stato di fatto | numero (n.) | Ordinari [uso/carico]; Accertamento [uso/carico] | Sez. 10 E25 |
| E13 | Unità immobiliari – progetto | numero (n.) | Ordinari [uso/carico]; Accertamento [uso/carico] | Sez. 10 E25 |
| E14 | Volumetria originaria | numero (mc) | Ordinari [parametri]; Condoni; Accertamento [parametri] | Sez. 10 E26; 13.8; 12 (ampliamenti %) |
| E15 | Incremento proposto / eseguito | numero (mc) | Ordinari [parametri]; Accertamento [parametri] | Sez. 10 E26 |
| E16 | Incremento sulla volumetria originaria (calcolato) | calcolato | — | Cella calcolata: non compilare |
| E17 | Incrementi già eseguiti negli ultimi 5 anni | numero (mc) | Ordinari [parametri] | Sez. 13.8; Allegato 1 NTA |
| E18 | Aumento di carico insediativo | elenco SN | Ordinari [uso/carico]; Accertamento [uso/carico] | Sez. 10 E27; 17.1 (zona rossa), 5 (PSAI) |
| E19 | Entità dell'aumento (abitanti/addetti/utenti) | numero (n.) | Ordinari [uso/carico]; Accertamento [uso/carico] – solo se aumenta il carico | Sez. 10 E27 |
| E20 | Aumento di volumi abitabili o del numero di unità abitative | elenco SN | Ordinari [uso/carico]; Accertamento [uso/carico] | Sez. 17.1 (divieto in zona rossa) |
| E21 | Interrati o seminterrati | elenco SN | Ordinari [parametri]; Accertamento [parametri] | Sez. 10 E28; 5 bis (Allegato A3) |
| E22 | Altezza netta dell'interrato/seminterrato | numero (m) | Ordinari [parametri]; Accertamento [parametri] – solo se ci sono interrati | Sez. 10 E28 |
| E23 | Quota dell'interrato rispetto al piano campagna | numero (m) | Ordinari [parametri]; Accertamento [parametri] – solo se ci sono interrati | Sez. 10 E28 |
| E24 | Quota del primo solaio rispetto al piano campagna | numero (m) | Ordinari [parametri]; Accertamento [parametri] | Sez. 5 bis (Allegato A3 §3) |
| E25 | Distanza minima dai confini | numero (m) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28.2; 6 |
| E26 | Distanza minima da edifici | numero (m) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28.2; 6 |
| E27 | Distanza minima dalle strade | numero (m) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28.2; 6; 8 n. 2 |
| E28 | Pareti finestrate nel fronte verso edifici/confine | elenco SN | Ordinari [parametri]; Accertamento [parametri] | Sez. 28.2 (Allegato art. 5) |
| E29 | Larghezza delle parti non finestrate | numero (m) | Ordinari [parametri]; Accertamento [parametri] – solo se le pareti non sono finestrate | Allegato 1 NTA art. 5 (limite di 3,00 m): sez. 28.4 n. 4 |
| E30 | Altezza delle parti non finestrate | numero (m) | Ordinari [parametri]; Accertamento [parametri] – solo se le pareti non sono finestrate | Allegato 1 NTA art. 5: sez. 28.4 n. 4 |
| E31 | Accordo con il confinante o parete cieca in confine | elenco SN | Ordinari [parametri]; Accertamento [parametri] | Sez. 28.2 |
| E32 | Superficie non edificata del lotto | numero (mq) | Ordinari [parametri]; Accertamento [parametri] | Allegato 1 NTA art. 17 (40%): sez. 8 n. 27 (NTA > RUEC) |
| E33 | Superficie già impermeabilizzata – stato di fatto | numero (mq) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28.2, 6 |
| E34 | Superficie impermeabilizzata – progetto | numero (mq) | Ordinari [parametri]; Accertamento [parametri] | Sez. 28.2, 6 |
| E35 | Posti auto pertinenziali previsti | numero (n.) | Ordinari [uso/carico]; Accertamento [uso/carico] | NTA art. 29 bis c. 5 (1 posto per unità; RUEC art. 36 c. 4 = 2: prevalgono le NTA): sez. 8 n. 1 |
| E36 | Ubicazione dei posti auto | elenco PARKUBIC | Ordinari [uso/carico]; Accertamento [uso/carico] | Sez. 17.1; 6 |
| E37 | Distanza dei posti auto dall'unità | numero (m) | Ordinari [uso/carico]; Accertamento [uso/carico] – solo se fuori lotto | Sez. 17.1 |
| E38 | Vincolo pertinenziale dei posti auto | elenco SN | Ordinari [uso/carico]; Accertamento [uso/carico] – solo se i posti auto sono previsti | Sez. 17.1 |
| E39 | Destinazione d'uso esistente (classi RUEC) | elenco USO | Ordinari [uso/carico]; Accertamento [uso/carico] | Sez. 6 (classi A-H); 14.1 (art. 23-ter) |
| E40 | Destinazione d'uso di progetto (classi RUEC) | elenco USO | Ordinari [uso/carico]; Accertamento [uso/carico] | Sez. 6; 14.1 |
| E41 | Mutamento d'uso verso l'abitativo | elenco SN | Ordinari [uso/carico]; Accertamento [uso/carico] | Sez. 17.1 (zona rossa: vietato, art. 6 c. 2 LR 21/2003) |
| E42 | Superficie di tettoie, autorimesse, volumi tecnici, ricoveri attrezzi | numero (mq) | Ordinari [parametri]; Accertamento [parametri] | Sez. 13.8 |
| E43 | Volume di tettoie, autorimesse, volumi tecnici, ricoveri attrezzi | numero (mc) | Ordinari [parametri]; Accertamento [parametri] | Sez. 13.8 |
| E44 | Demolizione e ricostruzione: corrispondenza di volume, sagoma e sedime | elenco SN | Ordinari [dem.-ric.]; Accertamento [dem.-ric.] | Sez. 13.8; 8 n. 27 (SCIA alternativa al PdC) |
| E45 | Intervento «tetto»: incremento di altezza al muro perimetrale | numero (m) | Ordinari [tetto]; Accertamento [tetto] | Allegato 1 NTA art. 14: sez. 28 |
| E46 | Intervento «tetto»: incremento di altezza al colmo | numero (m) | Ordinari [tetto]; Accertamento [tetto] | Sez. 28 |
| E47 | Intervento «tetto»: pendenza delle falde | numero (%) | Ordinari [tetto]; Accertamento [tetto] | Sez. 28 |
| E48 | Intervento «tetto»: larghezza del corpo di fabbrica | numero (m) | Ordinari [tetto]; Accertamento [tetto] | Sez. 28 |
| E49 | Intervento «tetto»: destinazione del volume ottenuto | testo | Ordinari [tetto]; Accertamento [tetto] | Sez. 28 |

### F – Pareri e titoli già acquisiti

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| F01 | Autorizzazione paesaggistica acquisita | elenco ACQ | Ordinari [parametri]; Condoni; Agibilità; Accertamento | Sez. 13.4, 32.2 |
| F02 | Scostamenti dal progetto autorizzato | elenco SN | Agibilità; Accertamento – solo se acquisita | Sez. 13.8 |
| F03 | Scostamento massimo (su altezza, distacchi, cubatura, superficie coperta, sedime) | numero (%) | Agibilità; Accertamento – solo se ci sono scostamenti | Sez. 13.8 |
| F04 | Nulla osta Ente Parco | elenco ACQ | Ordinari; Condoni; Accertamento – solo se dentro o contiguo al Parco | Sez. 32.2; L. 394/1991 art. 13 |
| F05 | Parere della Soprintendenza | elenco ACQ | Ordinari; Condoni; Accertamento – solo se bene culturale | Sez. 13.5 |
| F06 | Valutazione di incidenza (SIC/ZPS) | elenco ACQ | Ordinari; Condoni; Accertamento – solo se SIC/ZPS | Sez. 10 F37 |
| F07 | Autorizzazione sismica / deposito | elenco ACQ | Ordinari; Agibilità; Accertamento – solo se lavori strutturali | Sez. 14.7, 24, 36 |
| F08 | Autorizzazione sismica rilasciata prima dell'inizio dei lavori | elenco SN | Ordinari; Agibilità; Accertamento – solo se acquisita | Art. 94 DPR 380/2001: sez. 14.7 |
| F09 | Agibilità: finiture ultimate | elenco SN | Agibilità | Sez. 14.3 (art. 24) |
| F10 | Agibilità: collaudo statico | elenco SNNN | Agibilità | Sez. 14.3 |
| F11 | Agibilità: certificati degli impianti | elenco SN | Agibilità | Sez. 14.3 |
| F12 | Agibilità: requisiti di accessibilità | elenco SNNN | Agibilità | Sez. 14.3 |
| F13 | Agibilità: aggiornamento catastale | elenco SN | Agibilità | Sez. 14.8 |
| F14 | Agibilità: ricorso alle deroghe dell'art. 24 c. 5-bis (altezza, mq) | elenco SN | Agibilità | Sez. 14.3, 14.8 |

### H – Condoni (L. 47/85, L. 724/94, L. 326/03)

*Compilare solo se il procedimento è una sanatoria straordinaria.*

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| H00 | Sezione H pertinente? (procedimento di condono) | elenco SWITCH | Tutti | Interruttore: «No» = sezione saltata |
| H01 | Volume dell'opera abusiva (soglia dei 450 mc per la perizia) | numero (mc) | Condoni | Sez. 12.1, 30.4, 8 n. 16 |
| H02 | Ampliamento abusivo | numero (mc) | Condoni | Sez. 12.1 |
| H03 | Ampliamento sulla volumetria originaria (calcolato) | calcolato | — | Cella calcolata (usa la volumetria originaria del blocco E) |
| H04 | Nuova costruzione residenziale abusiva | numero (mc) | Condoni | Sez. 12.1; 21.1 (zona rossa) |
| H05 | Opere conformi alle norme e agli strumenti urbanistici | elenco SN | Condoni | Sez. 12.2 (tipologia 1 o 2); 30.1 |
| H06 | Domanda presentata nei termini (L. 326/03: sul modello entro il 31/03/2004) | elenco SN | Condoni | Sez. 12.5, 30.3 |
| H07 | Oblazione e oneri versati | elenco SN | Condoni | Sez. 12.5, 21.1; oneri anni precedenti: segnalati, non calcolati (8 n. 27) |
| H08 | Documentazione fotografica | elenco SN | Condoni | Sez. 12.5 |
| H09 | Perizia giurata e idoneità statica allegate | elenco SN | Condoni | Sez. 30.4, 8 n. 16 (450 mc o sempre: segnalare) |
| H10 | Parere dell'amministrazione preposta al vincolo | elenco PARERE | Condoni | Sez. 12.4, 12.9 |
| H11 | Possibilità di adeguamento antisismico | elenco SN | Condoni | Sez. 12.5 |
| H12 | Area demaniale o gravata da usi civici | elenco SN | Condoni | Sez. 12.3 |
| H13 | Area boscata o a pascolo percorsa dal fuoco negli ultimi 10 anni | elenco SN | Condoni | Sez. 12.3 |
| H14 | L. 326/03: tipologia di illecito (Allegato 1) | elenco TIPOL | Condoni – solo condono L. 326/03 | Sez. 30.1 |
| H15 | L. 326/03: lettera dell'art. 3 c. 1 DPR 380 (tipologie 3-6) | elenco LETT380 | Condoni – solo tipologie 3-6 | Sez. 30.1 |
| H16 | L. 326/03: opere completate al rustico entro il 31/03/2003 | elenco SN | Condoni – solo condono L. 326/03 | Sez. 21.1 (art. 3 c. 2 lett. b LR 10/2004) |
| H17 | L. 326/03: destinazione residenziale delle opere (zona rossa) | elenco SN | Condoni – solo condono L. 326/03 | Sez. 21.1 (art. 3 c. 2 lett. d) |
| H18 | L. 326/03: versamento del 30% di oblazione e anticipazione oneri (minimi 1.700 € e 500 €) | elenco SN | Condoni – solo condono L. 326/03 | Sez. 30.3 |
| H19 | L. 326/03: rate del 30/06 e 30/09/2004 versate | elenco SN | Condoni – solo condono L. 326/03 | Sez. 30.3 |
| H20 | L. 326/03: oneri definitivi versati | elenco ONERIDEF | Condoni – solo condono L. 326/03 | Sez. 30.3 (termine 31/12/2006) |
| H21 | L. 326/03: dichiarazione ex art. 4 L. 15/1968 presentata | elenco SN | Condoni – solo condono L. 326/03 | Sez. 30.3 |
| H22 | L. 326/03: integrazione del 30/09/2004 (denuncia catastale, ICI, tassa rifiuti, occupazione suolo) | elenco SN | Condoni – solo condono L. 326/03 | Sez. 30.3 |
| H23 | L. 47/85 e L. 724/94: dichiarazione sostitutiva ex art. 9 c. 2 LR 10/2004 presentata | elenco SN | Condoni – solo condono L. 47/85 o L. 724/94 | Sez. 21.2 (termine 31/12/2026) |
| H24 | L. 47/85 e L. 724/94: adeguamento sismico eseguito | elenco SN | Condoni – solo condono L. 47/85 o L. 724/94 | Sez. 21.2 |

### I – Leggi regionali (sottotetti, piscine, recinzioni, delocalizzazione)

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| I00 | Sezione I pertinente? (leggi regionali di settore) | elenco SWITCH | Tutti | Interruttore: «No» = sezione saltata |
| I01 | Tema regionale pertinente | elenco LRTIPO | Tutti | Sez. 16-22 |
| I02 | Edificio accatastato | elenco SN | Ordinari; Condoni; Accertamento | Sez. 18 (LR 13/2022) |
| I03 | Destinazione residenziale prevalente | elenco SN | Ordinari; Condoni; Accertamento | Sez. 18, 20 |
| I04 | Piscina interrata: superficie | numero (mq) | Ordinari; Condoni; Accertamento – solo piscina | LR 13/2022 art. 2 (piscine fino a 24 mq): sez. 18 |
| I05 | Piscina interrata: profondità | numero (m) | Ordinari; Condoni; Accertamento – solo piscina | Sez. 18 |
| I06 | Muro di cinta/recinzione: altezza | numero (m) | Ordinari; Condoni; Accertamento – solo recinzione | Sez. 18 |
| I07 | Muro di cinta/recinzione: materiale | elenco RECMAT | Ordinari; Condoni; Accertamento – solo recinzione | Sez. 18 |
| I08 | Sottotetto: data di esistenza | data | Ordinari; Condoni; Accertamento – solo sottotetto | Sez. 16.3, 19, 8 n. 8 |
| I09 | Sottotetto: collegamento all'unità sottostante | elenco SN | Ordinari; Condoni; Accertamento – solo sottotetto | Sez. 19 |
| I10 | Sottotetto: ampliamento di unità preesistente (in zona rossa) | elenco SN | Ordinari; Condoni; Accertamento – solo sottotetto | Sez. 16.3, 17 |
| I11 | Sottotetto: frazionamento in nuova unità | elenco SN | Ordinari; Condoni; Accertamento – solo sottotetto | Sez. 16.3 |
| I12 | Sottotetto: legittimità o sanatoria dell'edificio | elenco LEGIT | Ordinari; Condoni; Accertamento – solo sottotetto | Sez. 19 |
| I13 | Sottotetto: area esclusa dal Consiglio comunale | elenco SN | Ordinari; Condoni; Accertamento – solo sottotetto | LR 15/2000 art. 1 c. 2: sez. 8 n. 8 |
| I14 | Sottotetto: altezza media | numero (m) | Ordinari; Condoni; Accertamento – solo sottotetto | Art. 43 quater LR 16/2004 (≥ 2,10 m): sez. 16.3 |
| I15 | Sottotetto: altezza al colmo | numero (m) | Ordinari; Condoni; Accertamento – solo sottotetto | Sez. 16.3 |
| I16 | Sottotetto: altezza ai muri perimetrali | numero (m) | Ordinari; Condoni; Accertamento – solo sottotetto | Sez. 16.3 |
| I17 | Sottotetto: altezza minima di parete (≥ 1,40 m) | numero (m) | Ordinari; Condoni; Accertamento – solo sottotetto | Sez. 16.3 |
| I18 | Delocalizzazione: unità adibita a prima casa | elenco SN | Ordinari; Condoni; Accertamento – solo delocalizzazione | Sez. 20.3 (art. 11 bis LR 19/2009) |
| I19 | Delocalizzazione: quota di volumetria originaria destinata a uso non residenziale | numero (%) | Ordinari; Condoni; Accertamento – solo delocalizzazione | Sez. 17, 20.3 (almeno 50%) |

### K – Contributo di costruzione

*Delibera n. 17 del 15/12/2025 e foglio di calcolo dell'ufficio.*

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| K00 | Sezione K pertinente? (si calcola il contributo) | elenco SWITCH | Tutti | Interruttore: «No» = sezione saltata |
| K01 | Tipologia ai fini del K3 | elenco K3TIPO | Ordinari; Condoni; Accertamento | Sez. 29.3 |
| K02 | Classe di indice di fabbricabilità (If) | elenco IFCLASS | Ordinari; Condoni; Accertamento – solo residenziale | Sez. 29.3 |
| K03 | Volume «vuoto per pieno» (V) | numero (mc) | Ordinari; Condoni; Accertamento | Sez. 29.2 (≠ V.C. dell'Allegato 1 NTA: sez. 8 n. 15 d) |
| K04 | Superficie utile (Su) per il costo di costruzione | numero (mq) | Ordinari; Condoni; Accertamento | Sez. 29.5, 29.8 |
| K05 | Superficie non residenziale (Snr) | numero (mq) | Ordinari; Condoni; Accertamento | Sez. 29.5, 29.8 |
| K06 | Importo del computo metrico dei lavori (per l'esistente) | numero (€) | Ordinari; Condoni; Accertamento | Sez. 29.8 (prassi d'ufficio: 10%, computo metrico) |
| K07 | Impianti produttivi: numero di addetti | numero (n.) | Ordinari; Condoni; Accertamento – solo impianti produttivi | Sez. 29.4 (Up1) |
| K08 | Impianti produttivi: superficie S1 | numero (mq) | Ordinari; Condoni; Accertamento – solo impianti produttivi | Sez. 29.4 |
| K09 | Impianti produttivi: superficie S2 | numero (mq) | Ordinari; Condoni; Accertamento – solo impianti produttivi | Sez. 29.4 |
| K10 | Impianti produttivi: superficie S3 | numero (mq) | Ordinari; Condoni; Accertamento – solo impianti produttivi | Sez. 29.4 |
| K11 | Impianti produttivi: classe dell'industria (Cj) | testo | Ordinari; Condoni; Accertamento – solo impianti produttivi | Sez. 29.4 |

### L – Relazione paesaggistica

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| L00 | Sezione L pertinente? (serve l'autorizzazione paesaggistica) | elenco SWITCH | Tutti | Interruttore: «No» = sezione saltata |
| L01 | Procedimento paesaggistico | elenco PROCPAES | Ordinari; Condoni; Accertamento | Sez. 13.4, 13.6, 31; 8 n. 28 (vale la relazione semplificata del DPR 31) |
| L02 | Relazione paesaggistica allegata | elenco SN | Ordinari; Condoni; Accertamento | Sez. 31 |
| L03 | Contenuto: analisi dello stato attuale | elenco SN | Ordinari; Condoni; Accertamento | Sez. 31 (DPCM 12/12/2005) |
| L04 | Contenuto: livelli di tutela e beni culturali | elenco SN | Ordinari; Condoni; Accertamento | Sez. 31 |
| L05 | Contenuto: foto da 2 a 4 dal contesto e dai punti panoramici | elenco SN | Ordinari; Condoni; Accertamento | Sez. 31 |
| L06 | Contenuto: elaborati di progetto con prospetti e colori | elenco SN | Ordinari; Condoni; Accertamento | Sez. 31 |
| L07 | Contenuto: simulazione (rendering o fotoinserimento) | elenco SN | Ordinari; Condoni; Accertamento | Sez. 31 |
| L08 | Contenuto: effetti, mitigazioni e compensazioni | elenco SN | Ordinari; Condoni; Accertamento | Sez. 31 |
| L09 | Edificio esistente: rilievo dello stato di fatto e documentazione storica | elenco SN | Ordinari; Condoni; Accertamento | Sez. 31 |

### M – Edilizia libera (glossario DM 2/3/2018)

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| M00 | Sezione M pertinente? (edilizia libera o manufatti temporanei) | elenco SWITCH | Tutti | Interruttore: «No» = sezione saltata |
| M01 | Numero d'opera del glossario (1-58) | numero (n.) | Ordinari | Sez. 33.3 |
| M02 | Superficie coperta del manufatto (limite 30 mq per le «limitate dimensioni») | numero (mq) | Ordinari | Sez. 33; 8 n. 28 (30 mq = superficie coperta, solo pertinenze ammesse dalle NTA) |
| M03 | Altezza del manufatto | numero (m) | Ordinari | Sez. 33.3 |
| M04 | Tipo di ancoraggio | elenco ANCORAGGIO | Ordinari | Sez. 33.3 |
| M05 | Durata prevista (opere temporanee: max 180 giorni, non reiterabile) | numero (giorni) | Ordinari | Art. 6 lett. e-bis: sez. 8 n. 18 e 28 |
| M06 | Uso del manufatto | elenco MUSO | Ordinari | Sez. 33.3 |
| M07 | Interno al centro edificato | elenco SN | Ordinari | Sez. 33.3 |
| M08 | Indice di permeabilità dell'area | numero (%) | Ordinari | Sez. 33.3 |

### N – Piano di protezione civile

*Il piano non ha efficacia edilizia: serve solo per le segnalazioni di attenzione.*

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| N00 | Sezione N pertinente? (strade-cancello, aree di attesa, aumento residenti) | elenco SWITCH | Tutti | Interruttore: «No» = sezione saltata |
| N01 | Edificio/area coincidente con area di attesa, area di ricovero o sede COC | elenco SN | Ordinari; Accertamento | Sez. 34.3, 34.7 |
| N02 | Intervento che incide su carreggiata, sedime o accesso di una strada-cancello | elenco SN | Ordinari; Accertamento | Sez. 34.3 (attenzione, non diniego: 8 n. 28) |
| N03 | Quale strada-cancello (indicare la denominazione dal piano) | testo | Ordinari; Accertamento – solo se incide su una strada-cancello | Sez. 34.7 |
| N04 | Aumento dei residenti previsti (unità abitative, posti letto, strutture ricettive o di assistenza) | elenco SN | Ordinari; Accertamento | Sez. 34.7 |
| N05 | Entità dell'aumento | numero (n.) | Ordinari; Accertamento – solo se aumentano i residenti | Sez. 34.7 |

### S – Competenza sismica (LR 26/2025)

*Il Comune ha la commissione sismica attiva.*

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| S00 | Sezione S pertinente? (lavori su parti strutturali) | elenco SWITCH | Tutti | Interruttore: «No» = sezione saltata |
| S01 | Altezza strutturale (dal piano di posa delle fondazioni all'estradosso del torrino scala o al colmo) | numero (m) | Ordinari; Condoni; Accertamento | Sez. 36 (limite 16,50 m) |
| S02 | Profondità della parte interrata | numero (m) | Ordinari; Condoni; Accertamento | Sez. 36 (limite 4,00 m) |
| S03 | Istanza sismica già inoltrata al Genio Civile prima del 01/11/2025 | elenco SN | Ordinari; Condoni; Accertamento | Sez. 36 (LR 26/2025 art. 1 c. 1 ter) |

### P – Immobile con istanza di condono pendente (Delibera C.C. n. 22/2023)

*Solo per interventi su immobili con istanza non definita.*

| Codice | Campo | Tipo / lista | Obbligatorio per | Sintesi / nota |
|---|---|---|---|---|
| P00 | Sezione P pertinente? (condono L. 47/85 o L. 724/94 pendente sull'immobile) | elenco SWITCH | Tutti | Interruttore: «No» = sezione saltata |
| P01 | Legge dell'istanza di condono pendente | elenco LEGGEPEND | Tutti | Sez. 37.3 (non vale per la L. 326/2003) |
| P02 | Tipo di intervento richiesto | elenco PTIPO | Tutti | Sez. 37.2 |
| P03 | Cambio d'uso, frazionamento o accorpamento di unità | elenco SN | Tutti | Sez. 37.2 (condizioni ii) |
| P04 | Modifiche ai prospetti, alla superficie o al volume | elenco SN | Tutti | Sez. 37.2 (condizioni iii e iv) |
| P05 | Fotovoltaico/termico in copertura visibile da aree pubbliche o punti panoramici | elenco SN | Tutti | Sez. 37.2 n. 3 |
| P06 | Valutazione della sicurezza NTC (§ 8.3) acquisita | elenco SN | Tutti | Sez. 37.2 n. 2 |
| P07 | Autodichiarazione e perizia giurata presenti | elenco SN | Tutti | Sez. 37.2 n. 2-3 |
| P08 | Oblazione e oneri versati integralmente | elenco SN | Tutti | Sez. 37.3 (oneri: segnalati, non calcolati) |
| P09 | Ordinanze, sequestri o ricorsi pendenti | elenco SN | Tutti | Sez. 37.3 |
| P10 | Opere eseguite dopo la presentazione della domanda di condono | elenco SN | Tutti | Sez. 37.2 n. 3 |

## 3. Voci della sez. 10 della sintesi escluse dalla scheda

| Voce (sez. 10) | Motivo dell'esclusione |
|---|---|
| A4: interruzioni e preavviso di rigetto (14.2, 14.8) | Dati procedurali di competenza dell'ufficio (termini, silenzio-assenso): non incidono sul parere di fattibilità. |
| B5: zona A / centro storico (sì/no) | Deducibile: zona applicata (B04) = A. |
| B9: fondamento ed estremi del vincolo paesaggistico | Noto per Ottaviano (costante Z03: intero territorio, art. 134 lett. b/c, art. 142 lett. f, l; D.M. 02/09/1961). Resta il campo «vincolo rafforzato». |
| C18: zona sismica | Nota per Ottaviano (costante Z01: zona 2). |
| C18: sopraelevazione (sì/no) | Deducibile dalla qualificazione (A03). |
| C18: classe d'uso e categoria di sottosuolo | Dati di progetto strutturale: non cambiano esito, titolo o contributo; li verifica il progettista. |
| E31: superficie permeabile (% del lotto) | Superata: tra NTA e RUEC prevalgono le NTA (impermeabilizzazione 40% delle superfici non edificate, Allegato 1 art. 17; sez. 8 n. 27). Si raccolgono superficie non edificata e impermeabilizzata (E32-E34). |
| G: pericolosità frana (P1-P4) e idraulica (P1-P3), vulnerabilità topografica | Decisione dell'utente: la pericolosità PSAI non si considera (CLAUDE.md sez. 4). Il rischio atteso dei nuovi interventi è solo segnalato (sez. 8 n. 27). |
| G: falda sub-affiorante o conca endoreica | Solo informativo: dato di pericolosità, non considerata. |
| H: legge di riferimento del condono | Deducibile dal tipo di procedimento (A02: tre voci distinte). |
| H: zona omogenea A (tipologia 4) | Deducibile dalla zona applicata (B04). |
| I: territorio urbanizzato/rurale/città storica e qualifica dell'imprenditore agricolo, piano aziendale (RR 3/2025) | Decisione dell'utente (sez. 8 n. 10): vale il PUC; il RR 3/2025 si segnala con fonte e il territorio rurale si deduce dall'ambito. Il piano aziendale richiesto dalle NTA (artt. 8, 9, 11) è raccolto dal campo aggiunto D22. |
| J: Allegato B PSAI (tipologia di studio, relazione di compatibilità geologica allegata/firmata, interferenze interrati con falde e cavità, area declassata, collaudo) | Decisioni del 20/10/2026: la relazione di compatibilità geologica in R1-R4 non va richiesta; Allegato D non serve. Interrati e quota del primo solaio sono già nel blocco E. |
| N: microzona del piano di protezione civile | Il piano non ha efficacia edilizia e non c'è corrispondenza tra microzone e ambiti/fogli (sez. 34.5, 8 n. 20 e), quindi non cambia esito. |
| N: immobile nell'elenco dei beni culturali del piano | Ai fini del vincolo valgono i campi B (bene culturale). |
| O: Comune titolare delle funzioni sismiche | Noto: commissione sismica attiva (costante Z05). |
| O: estremi dell'istanza di condono pendente | Dato identificativo (numero di protocollo): non va nella scheda anonima; resta il solo tipo di legge (P01). |
| Estremi di titoli, autorizzazioni e provvedimenti (A4, D19, F37, F38) | Sostituiti da data o anno e dall'esito (acquisita / non acquisita): niente numeri di protocollo identificativi. |

**Voci fuse o riformulate** (non perse): B10 (bene culturale) con B7 (edificio tutelato) in B10; H «volumetria originaria» con E «volumetria originaria» (E14); H «opere in assenza o difformità» con D20 (D06-D07); H «domanda presentata nei termini» e L. 326/03 «domanda sul modello entro il 31/03/2004» in un solo campo (H06); H «data di ultimazione» con D21 (D09); K «edilizia convenzionata» e «zona B diretto o PUA» nelle liste K3 e zona (B-PUA); E29 «sottotetto» nel blocco I (sottotetti); L «voce dell'elenco» con B12 (B17); dati «estremi» dei titoli ridotti a data/esito.

## 4. Qualificazioni e flag (foglio Elenchi)

| Qualificazione | P parametri | C uso/carico | DR dem.-ric. | T tetto |
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

## 5. Controlli (foglio Controlli)

- **Riepilogo**: campi richiesti, compilati, **mancanti** (cella rossa se > 0), **non noti**.
- **13 segnalazioni automatiche** (non sostituiscono il parere): zona A-F applicata; vincolo paesaggistico anteriore/posteriore alle opere (condoni e accertamenti); incoerenze di interruttore (condono senza sezione H, edilizia libera senza M, sottotetto senza I, lavori strutturali senza S); competenza sismica (Comune o Genio Civile: 16,50 m, 4,00 m, istanza prima del 01/11/2025); rischio PSAI R3/R4; rischio atteso PSAI da segnalare senza concludere; Delibera C.C. 22/2023 (entro o oltre i limiti); date incoerenti; zona rossa (volumi abitabili o uso abitativo); zona non determinata per il K3.
- **Tabella per campo** con le regole ORD/CON/AGI/ACC, la pertinenza (interruttore), la condizione e lo stato (`ok`, `MANCANTE`, `NON NOTO`).

## 6. Scelte fatte, da confermare

1. **Date dei vincoli (B14-B16)**: obbligatorie solo per condoni e accertamenti di conformità (dove la data delle opere conta, sez. 12.3, 12.9), non per i titoli ordinari su opere future né per l'agibilità. Per «non applicabile» si scrive il testo.
2. **Collaudo, pratica 2**: «condono L. 47/85 pendente con intervento di manutenzione» è stata letta come **CILA di manutenzione straordinaria su immobile con istanza pendente** (blocco P, Delibera C.C. 22/2023), non come la pratica di condono stessa.
3. **Campo aggiunto D22** (piano di sviluppo aziendale): non è nella sez. 10, ma le NTA (artt. 8, 9, 11) lo richiedono per la nuova costruzione agricola.
4. **Qualificazioni e flag** (sez. 4): la ristrutturazione senza demolizione è trattata come P e C (parametri e carico richiesti); la manutenzione straordinaria e il restauro come né P né C; il cambio d'uso senza opere come solo C. Se vuoi più o meno campi per un tipo, si cambia nel foglio Elenchi.
5. **Stato di fatto → progetto**: per S.C., S.U., V.C., H, N.P. e unità immobiliari ci sono due celle (stato di fatto e progetto), richieste per ORD e ACC con flag P (le unità immobiliari con flag C).
6. **Blocco H** richiesto solo per i condoni (gruppo CON); **blocco L, K, S**: richiesti per ORD, ACC e CON quando l'interruttore è Sì; **blocco N**: solo ORD e ACC; **blocco P**: tutti i procedimenti. Se un interruttore è Sì per un gruppo non previsto, i campi restano facoltativi.
7. **Ambiti senza default**: art. 17 (disposizioni generali), artt. 32-35 e «Altro/non noto» non hanno zona nella tabella 35.0: la zona va indicata dal funzionario. ATS-2 non compare nella sez. 35.0 e non è nella lista.
8. **«B-PUA»** è nella lista delle zone come valore (colonna del foglio dell'ufficio, art. 16 parti libere): per il K3 la delibera n. 17/2025 non la contiene.
9. **Classi d'uso RUEC A-H** per le destinazioni; le categorie funzionali dell'art. 23-ter DPR 380 si ricavano nel parere.
10. **File consegnati ricalcolati con LibreOffice** (valori delle formule memorizzati, menu e formattazioni verificati); il font Aptos, se assente sul computer, è sostituito da Excel/LibreOffice.
11. **Non riletto sul PDF originale**: nessuna norma nuova è stata introdotta; i riferimenti sono quelli della sintesi v2.7.
