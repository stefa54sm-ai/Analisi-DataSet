# Analisi dei permessi delle applicazioni Android per il rilevamento di malware

## 1. Introduzione

Il presente progetto analizza un dataset relativo alle applicazioni Android, con l'obiettivo di studiare le differenze e le somiglianze tra applicazioni legittime (benign) e malevole (malware).

L'analisi si concentra sugli 86 permessi richiesti dalle applicazioni, utilizzati come caratteristiche per individuare configurazioni simili attraverso tecniche di clustering. I gruppi ottenuti verranno successivamente confrontati con le categorie originali del dataset.

Il progetto approfondisce inoltre la rappresentazione visiva dei risultati, applicando alcuni principi della Gestalt, come prossimità e somiglianza, per comprendere come la percezione visiva possa facilitare l'individuazione di raggruppamenti e relazioni tra i dati.

## 2. Descrizione del dataset

Per lo svolgimento del progetto è stato selezionato il dataset **NATICUSdroid (Android Permissions)**, disponibile presso l'**UCI Machine Learning Repository**.

Il dataset riguarda applicazioni Android rilasciate tra il 2010 e il 2019. Secondo i metadati del repository contiene 29.333 osservazioni, mentre la versione utilizzata nell'analisi comprende 29.332 applicazioni.

Ogni osservazione rappresenta un'applicazione ed è descritta da 86 caratteristiche (features), corrispondenti ai permessi Android richiesti. Ogni caratteristica assume un valore binario:

- **1** -> il permesso è presente tra quelli richiesti dall'applicazione;
- **0** -> il permesso non è presente tra quelli richiesti dall'applicazione.

È inoltre presente la variabile Result, che identifica la categoria dell'applicazione: legittima (benign, valore 0) o malevola (malware, valore 1).

Le 86 caratteristiche permettono di rappresentare ogni applicazione attraverso un profilo di permessi, rendendo possibile il confronto tra configurazioni diverse e l'individuazione di gruppi di applicazioni simili.

Nelle successive analisi di clustering verranno utilizzate esclusivamente le caratteristiche relative ai permessi. La variabile Result sarà mantenuta separata e utilizzata per confrontare i gruppi individuati con le categorie originali.

## 3. Analisi esplorativa dei dati (EDA)

L'analisi esplorativa dei dati (*Exploratory Data Analysis – EDA*) costituisce una fase preliminare fondamentale per comprendere la struttura del dataset e individuare eventuali caratteristiche che potrebbero influenzare le successive elaborazioni.

### 3.1 Distribuzione del numero di permessi per applicazione

La prima analisi riguarda il numero totale di permessi richiesti da ciascuna applicazione. Poiché le 86 caratteristiche sono codificate con valori binari (0 e 1), è sufficiente sommarle per ottenere il numero di permessi presenti in ogni applicazione.

Attraverso un istogramma sono state confrontate le distribuzioni delle applicazioni benigne e dei malware, per verificare eventuali differenze nel numero di autorizzazioni richieste.

Questa analisi permette di valutare se il numero complessivo di permessi possa contribuire a distinguere le due categorie o se sia necessario considerare anche la tipologia dei singoli permessi.
<img width="948" height="571" alt="image" src="https://github.com/user-attachments/assets/b696c476-4e1a-4217-8dec-0482e81aee50" />

### 3.2 Distribuzione delle classi

Per interpretare correttamente il confronto precedente, è stata analizzata la distribuzione della variabile target `Result`, con l'obiettivo di verificare la proporzione tra applicazioni benigne e applicazioni malevole.

Il controllo del bilanciamento delle classi è importante perché una distribuzione fortemente sbilanciata potrebbe influenzare l'interpretazione dei risultati e la valutazione delle successive elaborazioni.

L'analisi ha evidenziato una distribuzione pressoché equilibrata:

- Applicazioni benigne (0): 14.632, pari al 49,88%.
- Applicazioni malevole (1): 14.700, pari al 50,12%.

La differenza tra le due categorie risulta quindi contenuta. Entrambe sono rappresentate da un numero molto simile di osservazioni, condizione favorevole per effettuare confronti tra i rispettivi comportamenti.

La distribuzione è stata rappresentata attraverso un grafico a barre, che permette di confrontare visivamente il numero di applicazioni appartenenti alle due categorie.
<img width="724" height="493" alt="image" src="https://github.com/user-attachments/assets/8f73f5e4-edba-4f6d-96e3-88d5e43ded49" />

### 3.3 Confronto delle frequenze dei permessi tra le due categorie

Dopo l'analisi si è concentrata sulle frequenze dei singoli permessi nelle due categorie di applicazioni. Per ogni caratteristica è stata calcolata la proporzione di applicazioni benigne e malware che richiedono il relativo permesso.

Successivamente, è stata calcolata la differenza assoluta tra le due frequenze, selezionando i dieci permessi con le differenze maggiori. I risultati sono stati rappresentati attraverso un grafico a barre orizzontali, che permette di confrontare le due categorie.

Questa analisi consente di individuare i permessi maggiormente associati a una delle due categorie, senza tuttavia implicare un rapporto causale tra la loro presenza e la natura dell'applicazione.
<img width="1397" height="572" alt="image" src="https://github.com/user-attachments/assets/08b82038-855f-4708-8603-5d5faa568d17" />

### 3.4 Qualità e valori duplicati

È stata effettuata una verifica della qualità del dataset, controllando la presenza di valori mancanti, record duplicati e la codifica delle caratteristiche.

Non sono stati rilevati valori mancanti. Sono stati invece individuati:

- **21.841 record completamente duplicati**, identici sia nelle caratteristiche sia nella variabile `Result`.
- **21.937 profili di permessi ripetuti**, con la stessa configurazione delle 86 caratteristiche, indipendentemente dalla categoria.

Questa distinzione è importante perché applicazioni con identici profili di permessi possono appartenere a categorie diverse. La ripetizione di un profilo, quindi, non rappresenta necessariamente un'anomalia del dataset.

### 3.5 Profili di permessi associati a entrambe le categorie

Sono state individuate **96 configurazioni di permessi** condivise da applicazioni benigne e malware, per un totale di 5.624 applicazioni:

- 4.387 benigne;
- 1.237 malware.

Il risultato dimostra che applicazioni appartenenti a categorie diverse possono avere profili di permessi identici. Di conseguenza, i soli permessi non sono sempre sufficienti a distinguere le due categorie.

### 3.6 Analisi dei profili di permessi distinti

Dalle 29.332 applicazioni sono stati individuati **7.395 profili di permessi distinti**. Di questi, 5.347 compaiono una sola volta, mentre il più frequente è condiviso da 2.009 applicazioni.

Questi risultati evidenziano la presenza di numerose configurazioni ripetute, che potranno essere considerate nella preparazione dei dati per il clustering.

### 3.7 Analisi del numero di permessi per profilo

Per approfondire la struttura delle configurazioni presenti nel dataset, è stato analizzato il numero di permessi contenuti in ciascun profilo distinto. Poiché le 86 caratteristiche sono rappresentate da valori binari, la somma dei valori di ogni profilo consente di determinare quanti permessi sono attivi in quella specifica configurazione.

Dall'analisi emerge che il numero medio di permessi per profilo è pari a **12,24**, mentre la mediana è di **11 permessi**. Il valore minimo rilevato è 0, mentre quello massimo raggiunge 65 permessi.

La differenza tra il valore minimo e quello massimo evidenzia una notevole variabilità nella composizione dei profili. Alcune configurazioni non presentano alcun permesso attivo, mentre altre ne includono un numero considerevolmente elevato.

L'istogramma permette di osservare la distribuzione dei profili in base al numero di permessi richiesti, evidenziando le configurazioni più frequenti e quelle meno comuni.

Questa analisi è utile per comprendere la distribuzione delle caratteristiche nello spazio dei dati e costituisce un punto di partenza per le successive operazioni di clustering, nelle quali verranno confrontate le applicazioni in base alla somiglianza delle rispettive configurazioni di permessi.

## 4. Preparazione dei dati
<img width="1084" height="334" alt="image" src="https://github.com/user-attachments/assets/89423fd5-bf1c-49a7-aacb-d2162645f422" />


## 5. Scelta delle misure di distanza e degli algoritmi di clustering

## 6. Applicazione degli algoritmi di clustering

## 7. Analisi e interpretazione dei risultati

## 8. Visualizzazione dei risultati e principi della Gestalt

## 9. Conclusioni

## 10. Bibliografia e sitografia

Mathur, A., Podila, L. M., Kulkarni, K., Niyaz, Q., & Javaid, A. Y. (2021). *NATICUSdroid: A malware detection framework for Android using native and custom permissions*. Journal of Information Security and Applications, 58, 102696.

UCI Machine Learning Repository. *NATICUSdroid (Android Permissions)*. https://doi.org/10.24432/C5FS64
