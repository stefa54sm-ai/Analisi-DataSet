# Analisi dei permessi delle applicazioni Android per il rilevamento di malware

## 1. Introduzione

Il progetto analizza un dataset di applicazioni Android per studiare le differenze tra applicazioni legittime (*benign*) e malevole (*malware*).

L'analisi utilizza gli 86 permessi richiesti dalle applicazioni come caratteristiche per individuare gruppi di configurazioni simili attraverso algoritmi di clustering. I gruppi ottenuti verranno confrontati con le categorie originali del dataset.

I risultati saranno inoltre rappresentati visivamente, applicando alcuni principi della Gestalt, come prossimità e somiglianza.

## 2. Descrizione del dataset

Per il progetto è stato selezionato il dataset **NATICUSdroid (Android Permissions)**, disponibile presso l'**UCI Machine Learning Repository**.

Il dataset riguarda applicazioni Android rilasciate tra il 2010 e il 2019. I metadati riportano 29.333 osservazioni, mentre la versione utilizzata contiene 29.332 applicazioni.

Ogni applicazione è descritta da 86 caratteristiche (*features*), corrispondenti ai permessi richiesti. Ogni caratteristica assume valore 1 se il permesso è presente e 0 se è assente.

La variabile `Result` identifica la categoria dell'applicazione: benigna (0) o malware (1).

Per il clustering verranno utilizzate le 86 caratteristiche, mentre `Result` resterà separata e servirà per confrontare i gruppi individuati con le categorie originali.

## 3. Analisi esplorativa dei dati (EDA)

L'analisi esplorativa è stata utilizzata per esaminare la distribuzione dei dati, le caratteristiche dei permessi e la presenza di configurazioni ripetute.

È stato inizialmente calcolato il numero totale di permessi richiesti da ogni applicazione, sommando le 86 caratteristiche binarie. Le distribuzioni delle applicazioni benigne e dei malware sono state confrontate attraverso un istogramma.

<Inserire istogramma del numero di permessi per applicazione>

La distribuzione della variabile `Result` mostra 14.632 applicazioni benigne (49,88%) e 14.700 malware (50,12%). Le due categorie risultano quindi pressoché bilanciate.

<Inserire grafico della distribuzione delle classi>

Successivamente, sono state calcolate le frequenze relative dei singoli permessi nelle due categorie. Sono stati selezionati i dieci permessi con la maggiore differenza assoluta tra le frequenze, rappresentandoli attraverso un grafico a barre orizzontali. Le differenze osservate indicano una diversa distribuzione dei permessi tra le categorie, senza implicare rapporti causali.

<Inserire grafico delle frequenze dei dieci permessi>

La verifica della qualità del dataset non ha rilevato valori mancanti. Sono stati individuati 21.841 record completamente duplicati, identici sia nelle caratteristiche sia nella variabile `Result`, e 21.937 profili di permessi ripetuti, indipendentemente dalla categoria.

Sono state inoltre individuate 96 configurazioni di permessi condivise da applicazioni benigne e malware, associate complessivamente a 5.624 applicazioni: 4.387 benigne e 1.237 malevole.

Considerando le configurazioni distinte, il dataset presenta 7.395 profili differenti. Di questi, 5.347 compaiono una sola volta, mentre il più frequente è condiviso da 2.009 applicazioni.

Infine, è stato calcolato il numero di permessi attivi per ciascun profilo distinto. La media è di 12,24 permessi, la mediana di 11, il minimo di 0 e il massimo di 65.

<Inserire istogramma del numero di permessi per profilo>

## 4. Preparazione dei dati

## 5. Scelta delle misure di distanza e degli algoritmi di clustering

## 6. Applicazione degli algoritmi di clustering

## 7. Analisi dei risultati

## 8. Visualizzazione e principi della Gestalt

## 9. Conclusioni

## 10. Bibliografia e sitografia

Mathur, A., Podila, L. M., Kulkarni, K., Niyaz, Q., & Javaid, A. Y. (2021). *NATICUSdroid: A malware detection framework for Android using native and custom permissions*. Journal of Information Security and Applications, 58, 102696.

UCI Machine Learning Repository. *NATICUSdroid (Android Permissions)*. https://doi.org/10.24432/C5FS64
