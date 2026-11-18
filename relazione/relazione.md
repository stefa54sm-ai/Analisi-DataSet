# Analisi dei permessi delle applicazioni Android per il rilevamento di malware

## 1. Introduzione

Il presente progetto si propone di analizzare un insieme di dati relativo alle applicazioni Android, con particolare attenzione alla distinzione tra applicazioni legittime (*benign*) e applicazioni malevole (*malware*).

L'obiettivo principale è studiare le caratteristiche delle applicazioni attraverso tecniche di analisi dei dati e di apprendimento automatico, al fine di individuare eventuali somiglianze, differenze e raggruppamenti tra le osservazioni.

Il lavoro si concentra in particolare sui permessi richiesti dalle applicazioni Android, ovvero le autorizzazioni necessarie per accedere a determinate funzionalità del dispositivo o alle informazioni dell'utente. L'insieme dei permessi richiesti può infatti fornire indicazioni sulle caratteristiche di un'applicazione e contribuire all'individuazione di comportamenti potenzialmente sospetti.

Un aspetto centrale del progetto consiste nell'applicazione di tecniche di analisi dei dati affrontate durante il corso di *Principi e Modelli della Percezione*. Attraverso l'utilizzo di algoritmi di clustering, si intende verificare se sia possibile individuare gruppi di applicazioni caratterizzati da configurazioni di permessi simili, analizzando successivamente la loro relazione con le categorie originali.

L'analisi computazionale sarà affiancata dalla rappresentazione visiva dei dati. In particolare, verranno presi in considerazione alcuni principi della Gestalt, come prossimità, somiglianza e raggruppamento, per osservare come la disposizione e la codifica visiva delle informazioni possano facilitare il riconoscimento di schemi e relazioni.

Il progetto mira quindi a collegare l'organizzazione matematica dei dati con i meccanismi attraverso cui l'essere umano percepisce e interpreta le informazioni visive, mettendo in relazione le tecniche di raggruppamento automatico con la loro rappresentazione e interpretazione percettiva.

## 2. Descrizione del dataset

Per lo svolgimento del progetto è stato selezionato il dataset **NATICUSdroid (Android Permissions)**, disponibile presso l'**UCI Machine Learning Repository**.

Secondo i metadati del repository, il dataset è descritto come composto da 29.333 osservazioni. La versione scaricata e utilizzata nell'analisi contiene invece 29.332 applicazioni Android. I dati riguardano applicazioni rilasciate nel periodo compreso tra il 2010 e il 2019.

Ogni osservazione rappresenta una singola applicazione Android, mentre le caratteristiche (*features*) descrivono la presenza o l'assenza di specifici permessi richiesti dall'applicazione.

Il dataset comprende 86 caratteristiche, rappresentate attraverso variabili numeriche intere con codifica binaria:

- **1:** il permesso è presente tra quelli richiesti dall'applicazione;
- **0:** il permesso non è presente tra quelli richiesti dall'applicazione.

Oltre alle caratteristiche, il dataset contiene una variabile target denominata `Result`, che identifica la categoria dell'applicazione, distinguendo tra applicazioni legittime e applicazioni malevole.

La struttura dei dati consente di rappresentare ogni applicazione come un vettore composto da 86 valori binari. È quindi possibile confrontare le applicazioni sulla base dei permessi condivisi e delle differenze tra le rispettive configurazioni.

Questa tipologia di dati risulta particolarmente interessante per l'applicazione di tecniche di clustering, che permettono di individuare gruppi di osservazioni caratterizzati da proprietà simili, senza conoscere preventivamente la loro categoria di appartenenza.

La variabile `Result` verrà considerata durante l'analisi esplorativa e nella valutazione dei risultati. Nel caso delle tecniche di clustering non supervisionato, invece, non verrà utilizzata come caratteristica in ingresso agli algoritmi, ma servirà successivamente per confrontare i gruppi individuati con le categorie originali.

## 3. Analisi esplorativa dei dati (EDA)

L'analisi esplorativa dei dati (*Exploratory Data Analysis – EDA*) costituisce una fase preliminare fondamentale per comprendere la struttura del dataset e individuare eventuali caratteristiche che potrebbero influenzare le successive elaborazioni.

Prima di applicare gli algoritmi di clustering, è necessario esaminare la distribuzione delle osservazioni, verificare la qualità dei dati e comprendere come sono organizzate le caratteristiche disponibili.

L'analisi è stata articolata in diverse fasi, ciascuna dedicata a un aspetto specifico del dataset.

### 3.1 Distribuzione del numero di permessi per applicazione

La prima analisi riguarda il numero complessivo di permessi richiesti da ciascuna applicazione Android.

Poiché le 86 caratteristiche sono rappresentate attraverso valori binari, è possibile calcolare il numero totale di permessi sommando i valori presenti in ogni riga del dataset. In questo modo, a ogni applicazione viene associato un valore che indica quanti permessi risultano richiesti.

L'obiettivo è confrontare la distribuzione del numero di permessi tra applicazioni benigne e malware, verificando se le due categorie presentino comportamenti differenti.

Per effettuare questo confronto è stato utilizzato un istogramma, nel quale le due categorie sono rappresentate attraverso colori distinti. La sovrapposizione delle distribuzioni consente di osservare eventuali differenze nella frequenza e nella concentrazione delle applicazioni in corrispondenza dei diversi numeri di permessi.

Questa prima osservazione permette di valutare se il numero complessivo di autorizzazioni possa rappresentare un elemento utile per distinguere le due categorie, oppure se sia necessario considerare anche la tipologia specifica dei permessi richiesti.

### 3.2 Distribuzione delle classi

Per interpretare correttamente il confronto precedente, è stata analizzata la distribuzione della variabile target `Result`, con l'obiettivo di verificare la proporzione tra applicazioni benigne e applicazioni malevole.

Il controllo del bilanciamento delle classi è importante perché una distribuzione fortemente sbilanciata potrebbe influenzare l'interpretazione dei risultati e la valutazione delle successive elaborazioni.

L'analisi ha evidenziato una distribuzione pressoché equilibrata:

- Applicazioni benigne (0): 14.632, pari al 49,88%.
- Applicazioni malevole (1): 14.700, pari al 50,12%.

La differenza tra le due categorie risulta quindi contenuta. Entrambe sono rappresentate da un numero molto simile di osservazioni, condizione favorevole per effettuare confronti tra i rispettivi comportamenti.

La distribuzione è stata rappresentata attraverso un grafico a barre, che permette di confrontare visivamente il numero di applicazioni appartenenti alle due categorie.

### 3.3 Confronto delle frequenze dei permessi tra le due categorie

Dopo aver verificato la distribuzione delle classi, l'analisi si è concentrata sulle singole caratteristiche, con l'obiettivo di individuare quali permessi presentino le differenze di frequenza più significative tra applicazioni benigne e malware.

Per ogni permesso è stata calcolata la frequenza relativa all'interno delle due categorie. Considerando che le caratteristiche sono binarie, la media di ciascuna colonna rappresenta la proporzione di applicazioni che richiedono il relativo permesso.

Successivamente, è stata calcolata la differenza assoluta tra le frequenze delle due categorie per ciascun permesso. Sulla base di questo valore sono stati selezionati i dieci permessi che presentano la maggiore differenza.

I risultati sono stati rappresentati attraverso un grafico a barre orizzontali, nel quale è possibile confrontare direttamente la frequenza di ogni permesso nelle due categorie.

Questa analisi permette di individuare quali autorizzazioni risultino maggiormente rappresentate in una categoria rispetto all'altra. È importante precisare che una differenza di frequenza non implica necessariamente un rapporto causale con la natura malevola dell'applicazione, ma rappresenta un'indicazione statistica utile per approfondire le caratteristiche del dataset.

### 3.4 Verifica della qualità e della struttura dei dati

Una volta analizzate le principali caratteristiche delle applicazioni, è stata effettuata una verifica della qualità del dataset.

Questa fase ha riguardato il controllo dei valori mancanti, dei record duplicati e della codifica utilizzata per rappresentare i permessi.

L'assenza di valori mancanti consente di procedere con le successive elaborazioni senza dover ricorrere a tecniche di imputazione dei dati.

Un ulteriore aspetto considerato riguarda la presenza di osservazioni duplicate. In particolare, sono state effettuate due verifiche distinte:

- **Record completamente duplicati:** osservazioni che presentano gli stessi valori sia nelle caratteristiche sia nella variabile target.
- **Profili di permessi ripetuti:** osservazioni che presentano la stessa configurazione delle 86 caratteristiche, indipendentemente dalla categoria di appartenenza.

L'analisi ha individuato 21.841 record completamente duplicati e 21.937 profili di permessi ripetuti.

La distinzione tra queste due tipologie è importante, poiché applicazioni con gli stessi permessi non necessariamente appartengono alla stessa categoria. Per questo motivo, la presenza di profili ripetuti non può essere interpretata automaticamente come un'anomalia del dataset.

### 3.5 Profili di permessi associati a entrambe le categorie

Per approfondire il fenomeno delle configurazioni ripetute, è stata effettuata un'ulteriore analisi finalizzata a individuare i profili di permessi associati sia ad applicazioni benigne sia a malware.

L'analisi ha permesso di individuare 96 configurazioni distinte condivise da applicazioni appartenenti a entrambe le categorie.

Complessivamente, questi profili coinvolgono 5.624 applicazioni, così distribuite:

- 4.387 applicazioni benigne;
- 1.237 applicazioni malevole.

Questo risultato evidenzia un aspetto significativo della struttura del dataset: applicazioni appartenenti a categorie differenti possono presentare configurazioni di permessi perfettamente identiche.

Di conseguenza, le sole informazioni relative ai permessi non sono sempre sufficienti per distinguere un'applicazione legittima da una malevola.

Dal punto di vista del clustering, questa osservazione assume particolare importanza. Due applicazioni con lo stesso profilo di permessi occupano infatti la stessa posizione nello spazio delle caratteristiche, anche quando appartengono a classi differenti.

Questo fenomeno dovrà essere considerato durante l'interpretazione dei gruppi individuati dagli algoritmi, poiché la somiglianza tra le caratteristiche non implica necessariamente una corrispondenza tra le categorie originali.

### 3.6 Analisi dei profili di permessi distinti

Per comprendere meglio la struttura delle osservazioni, è stata effettuata un'ulteriore analisi considerando esclusivamente le configurazioni di permessi differenti.

A fronte di 29.332 applicazioni complessive, sono stati individuati 7.395 profili di permessi distinti. I restanti 21.937 record rappresentano ripetizioni di configurazioni già presenti nel dataset.

Questo risultato mostra che un numero considerevole di applicazioni condivide esattamente la stessa combinazione di permessi.

L'analisi delle frequenze dei profili ha inoltre evidenziato che 5.347 configurazioni compaiono una sola volta, mentre il profilo più frequente è associato a 2.009 applicazioni.

La distribuzione evidenzia quindi la presenza di numerosi profili poco frequenti e di alcune configurazioni condivise da un numero elevato di applicazioni.

Questa caratteristica è particolarmente rilevante per la fase successiva del progetto. La possibilità di lavorare sui profili distinti, anziché su tutte le singole osservazioni, potrebbe consentire di ridurre il numero di configurazioni da elaborare, mantenendo un'informazione sulla loro frequenza. Sarà tuttavia necessario conservare anche le informazioni relative alle categorie associate a ciascun profilo.

### 3.7 Analisi del numero di permessi per profilo

[Da completare dopo il calcolo del numero medio, minimo, massimo e mediano dei permessi e l'osservazione del relativo istogramma.]

## 4. Preparazione dei dati

## 5. Scelta delle misure di distanza e degli algoritmi di clustering

## 6. Applicazione degli algoritmi di clustering

## 7. Analisi e interpretazione dei risultati

## 8. Visualizzazione dei risultati e principi della Gestalt

## 9. Conclusioni

## 10. Bibliografia e sitografia

Mathur, A., Podila, L. M., Kulkarni, K., Niyaz, Q., & Javaid, A. Y. (2021). *NATICUSdroid: A malware detection framework for Android using native and custom permissions*. Journal of Information Security and Applications, 58, 102696.

UCI Machine Learning Repository. *NATICUSdroid (Android Permissions)*. https://doi.org/10.24432/C5FS64
