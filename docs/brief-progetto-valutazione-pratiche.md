# Progetto: sistema di supporto alla valutazione urbanistica delle pratiche edilizie
**Comune di Ottaviano – Settore Urbanistica ed Edilizia Privata**
Documento di sintesi del contesto, dei requisiti e delle soluzioni individuate. Redatto il 13/10/2026.

> Questo documento è pensato per essere dato a qualsiasi assistente AI (Claude, ChatGPT, ecc.) come contesto iniziale, in modo da poter confrontare i pareri prodotti da sistemi diversi sulle stesse pratiche.

---

## 1. Contesto

- **Utente**: Milvio Nocerino, ingegnere civile (laurea triennale), istruttore tecnico cat. C (in attesa di progressione a cat. D) presso l'Ufficio Tecnico del Comune di Ottaviano, settore Urbanistica ed Edilizia Privata.
- **Situazione**: è l'unica persona in ufficio e svolge da solo tutti i procedimenti.
- **Obiettivo generale**: velocizzare e automatizzare al massimo i procedimenti, delegando agli strumenti la parte oggettiva (verifiche, controlli, redazione) e riservando a sé la parte decisionale e intellettuale.
- **Strumento in sviluppo**: un portale/app interna all'ufficio per ottimizzare il flusso di lavoro.
- **Linea guida trasversale**: ottimizzazione dei costi di utilizzo. L'assistente deve suggerire quando cambiare modello e/o aprire una nuova chat per ridurre i costi.

## 2. Procedimenti da automatizzare

Permessi di costruire, SCIA, CILA, condoni edilizi, agibilità.
**Esclusi**: certificati di destinazione urbanistica e accessi agli atti.

I volumi mensili e i tempi medi per pratica **non influiscono** sul progetto: le pratiche verranno fornite una alla volta.

## 3. Flusso attuale

- Le pratiche arrivano dal portale **SUEDI**.
- I dati stanno sul server del Comune.
- L'app in sviluppo è quella usata dall'ufficio.
- I documenti da valutare saranno letti da un **account Google Drive dedicato**.

## 4. Cosa deve fare il sistema

**Funzione richiesta**: valutazione urbanistica di una pratica e **parere di fattibilità** dell'intervento previsto.

- Esito: **parere positivo o negativo** sull'istanza.
- In caso di parere negativo: **eventuali soluzioni alternative** per rendere l'intervento fattibile.
- Il parere deve contenere motivazione e riferimenti normativi puntuali.

**Decisioni riservate all'utente**: esito finale e firma. Il sistema produce solo un'istruttoria di supporto. Il PDF del parere deve riportare esplicitamente questa precisazione.

## 5. Quadro normativo di riferimento

### Nazionale
- DPR 380/2001 (Testo unico edilizia)
- DPR 31/2017 (interventi esclusi o semplificati in autorizzazione paesaggistica)
- D.Lgs. 42/2004 (Codice dei beni culturali e del paesaggio)
- Da integrare, se utili: DM 1444/1968, D.Lgs. 222/2016

### Vincolo paesaggistico
**L'intero territorio di Ottaviano è soggetto a vincolo paesaggistico** ai sensi dell'art. 134, lett. b) e c), del D.Lgs. 42/2004.

### Regionale (Campania)
- L.R. 21/2003
- L.R. 16/2004 ("Norme sul governo del territorio")
- L.R. 5/2024 (in vigore dal 30 aprile 2024)
- Regolamento Regionale 3/2025 del 6 ottobre 2025 (BURC n. 71 dell'8 ottobre 2025), che completa la L.R. 5/2024
- Da verificare se applicabile: L.R. 19/2009 (Piano casa)

### Rischio frana e idrico
Normativa dell'Autorità di Bacino della Campania Centrale, **delibera n. 1 del 25/02/2015**, con norme di attuazione e carte di rischio.

### Locale
- Nota del Comune (da fornire)
- **RUEC** di Ottaviano
- **PUC**: NTA e tavole di zonizzazione

### Da verificare se applicabili al territorio
Parco Nazionale del Vesuvio e zona rossa vesuviana, vincolo idrogeologico, classificazione sismica.

> Importante: i testi vanno forniti in PDF con testo selezionabile (non scansioni). Le citazioni normative devono essere verificate dall'utente sul testo vigente: un assistente AI non deve citare articoli a memoria se il testo non è stato fornito.

## 6. Il flusso di lavoro progettato

### Struttura su Google Drive
Tre cartelle:
1. **Da valutare**: schede anonime in attesa
2. **Valutate**: schede già esaminate
3. **Pareri**: PDF dei pareri emessi

### Ciclo di valutazione
1. L'utente compila la **scheda anonima** e la salva in "Da valutare".
2. La valutazione parte in uno di questi modi:
   - **su comando dell'utente** (nell'app, tramite pulsante "Valuta pratica"), oppure
   - **ad intervalli automatici**, controllando la cartella.
3. Il sistema valuta e genera il **parere in PDF** con la grafica istituzionale del Comune, salvato in "Pareri".
4. La scheda viene spostata in "Valutate" con lo stesso identificativo del parere.
5. **Una scheda valutata non deve mai essere rivalutata.**
6. Se la cartella "Da valutare" è vuota, il processo si ferma subito (costo minimo).

### Privacy
- Le schede riportano **solo dati oggettivi dell'intervento**, senza dati personali di richiedenti, tecnici o persone coinvolte.
- L'utente compila la scheda a mano e la fornisce anonima tramite l'account Google dedicato.
- Se in futuro la compilazione diventasse automatica, l'anonimizzazione dovrà avvenire **prima** dell'invio dei dati a qualunque servizio esterno.
- **Da verificare**: compatibilità dell'invio delle schede a un servizio AI esterno con il regolamento comunale sull'IA e con il parere del DPO.

## 7. Scheda anonima

**Non esiste ancora un modello.** L'utente lo sta strutturando. Richiesta: una volta fornita e studiata la normativa, l'assistente propone una struttura di scheda (campi, formato, criteri di valutazione). Il parere sarà affidabile solo quanto la scheda è completa.

Campi da considerare (proposta iniziale, da validare):
- tipo di procedimento (PdC, SCIA, CILA, condono, agibilità)
- tipo di intervento e destinazione d'uso
- zona di PUC e parametri urbanistici (indici, altezze, distanze, superfici)
- vincoli presenti (paesaggistico, frana, idrico, altri)
- stato legittimo dell'immobile
- eventuali difformità
- riferimenti catastali non identificativi della persona

## 8. Soluzione tecnica scelta

**Utilizzo di API escluso per ora** (a pagamento). Prima si testa il funzionamento, poi si valuta.

### Fase attuale: tutto dentro l'assistente AI, flusso semi-manuale
- Valutazione su comando: la scheda viene caricata o indicata e l'assistente produce parere e PDF.
- Lettura/spostamento file su Drive tramite connettore, dove disponibile.
- Controllo automatico periodico: possibile solo se il piano in uso include attività programmate. Verificare quanti messaggi consumano.
- Il pulsante "Valuta pratica" nell'app **non può invocare direttamente** l'assistente senza API: in questa fase può solo aprire la chat o copiare un comando pronto da incollare.

### Evoluzione futura
Se il test dà esito positivo, si può passare all'API: scheda e flusso restano identici, cambia solo il "motore" di valutazione. Con l'API il pulsante nell'app chiamerebbe direttamente il servizio.

### Contenimento dei costi
1. Caricare la normativa **una sola volta** in un progetto/spazio dedicato.
2. **Una chat per pratica** (o per piccoli gruppi): le chat lunghe consumano di più.
3. Modello economico per i casi di routine, modello più potente solo per i casi dubbi o complessi.
4. Creare un **documento di sintesi normativa** con le sole parti applicabili, per ridurre il testo da rileggere.
5. Lavorare in chat separate: una per la normativa, una per il modulo di valutazione, una per le pratiche reali.

## 9. Grafica del parere

Il parere è in **PDF** con la grafica istituzionale del Comune di Ottaviano:

| Elemento | Valore |
|---|---|
| Blu primario | #1F3864 |
| Blu secondario | #2E5395 |
| Blu chiaro / link | #4A7FB5 |
| Accento teal (uso sparso) | #2A9D8F |
| Testo | #262626 |
| Grigio medio | #6B6B6B |
| Sfondo chiaro | #F2F4F7 |
| Font documenti Word | Aptos |
| Font applicazioni web | Inter o sans-serif di sistema |
| Stile applicazioni | bordi arrotondati 8px, spaziatura generosa |

Stemma/logo ufficiale del Comune: da fornire.

## 10. Piano di lavoro

1. **Raccolta normativa**: caricamento di PUC (NTA e tavole), RUEC, delibera Autorità di Bacino e nota comunale per primi; poi norme nazionali e regionali.
2. **Struttura della scheda**: proposta dell'assistente dopo lo studio della normativa.
3. **Modulo di valutazione**: costruzione del flusso (lettura scheda, controlli, parere, PDF, archiviazione).
4. **Test su pratiche reali già concluse**: confronto tra parere del sistema e decisione dell'utente.

## 11. Confronto tra assistenti AI

L'utente intende dare lo stesso materiale a più assistenti (Claude e ChatGPT) per verificare se i pareri coincidono. Per un confronto valido:

- Dare **gli stessi identici documenti** normativi e **la stessa scheda** a entrambi.
- Usare **lo stesso prompt** di valutazione.
- Partire da **pratiche già concluse** di cui l'utente conosce l'esito, così da avere un riferimento.
- Registrare per ogni pratica: esito (positivo/negativo), norme citate, alternative proposte, eventuali errori o citazioni non verificabili.
- Verificare che **ogni articolo citato esista nel testo fornito**.
- Valutare anche costo e tempo di ciascun sistema.

Tabella di confronto suggerita:

| Pratica | Esito utente | Esito AI 1 | Esito AI 2 | Norme citate corrette? | Alternative utili? | Note |
|---|---|---|---|---|---|---|
| | | | | | | |

## 12. Punti ancora aperti

- [ ] Modello di scheda anonima (da proporre dopo lo studio della normativa)
- [ ] Fornitura dei testi normativi, in particolare PUC/NTA, RUEC, delibera Autorità di Bacino, nota comunale
- [ ] Conferma dell'applicabilità di Parco del Vesuvio, zona rossa, vincolo idrogeologico, classificazione sismica
- [ ] Verifica con regolamento AI comunale e DPO dell'uso di servizi AI esterni
- [ ] Modalità di avvio: comando manuale, intervalli automatici o entrambi
- [ ] Stemma ufficiale del Comune
- [ ] Verifica del piano in uso: attività programmate e limiti di utilizzo

---

## Istruzioni per l'assistente AI che riceve questo documento

1. Leggi questo documento come contesto del progetto.
2. Non esprimere pareri di fattibilità finché non hai ricevuto i testi normativi pertinenti.
3. Cita solo articoli presenti nei testi forniti. Se un riferimento non è verificabile, dillo.
4. Il parere è un'istruttoria di supporto: la decisione e la firma spettano al funzionario.
5. Se i dati della scheda sono insufficienti, elenca cosa manca invece di supporre.
6. Segnala quando conviene cambiare modello o aprire una nuova chat per contenere i costi.
