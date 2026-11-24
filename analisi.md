# Analisi dei permessi delle applicazioni Android per il rilevamento di malware

## 1. Introduzione

Il progetto analizza un dataset di applicazioni Android per studiare le differenze tra applicazioni legittime (*benign*) e malevole (*malware*).

L'analisi utilizza gli 86 permessi richiesti dalle applicazioni come caratteristiche per individuare gruppi di configurazioni simili attraverso algoritmi di clustering. I gruppi ottenuti verranno confrontati con le categorie originali del dataset.

I risultati saranno inoltre rappresentati visivamente, applicando alcuni principi della Gestalt, come prossimità e somiglianza.

## 2. Descrizione del dataset

Per il progetto è stato selezionato il dataset **NATICUSdroid (Android Permissions)**, disponibile presso l'**UCI Machine Learning Repository**. Il dataset riguarda applicazioni Android rilasciate tra il 2010 e il 2019.

Secondo i metadati del repository, il dataset contiene 29.333 osservazioni, mentre la versione utilizzata nell'analisi comprende 29.332 applicazioni.

Ogni riga del dataset rappresenta una singola applicazione Android ed è definita come **record** (osservazione). Ogni applicazione è descritta attraverso 86 caratteristiche (*features*), corrispondenti ai permessi Android richiesti. Ciascuna caratteristica assume un valore binario:

- **1**: il permesso è presente tra quelli richiesti dall'applicazione;
- **0**: il permesso non è presente.

La combinazione degli 86 valori costituisce il **profilo di permessi** dell'applicazione. Applicazioni diverse possono quindi avere lo stesso profilo, se presentano esattamente la stessa configurazione di autorizzazioni.

Il dataset comprende inoltre una variabile denominata `Result`, che rappresenta la **variabile target** e identifica la categoria dell'applicazione:

- **0**: applicazione legittima (*benign*);
- **1**: applicazione malevola (*malware*).

Nel progetto, le 86 caratteristiche saranno utilizzate come dati di ingresso per gli algoritmi di clustering, mentre `Result` verrà mantenuta separata e utilizzata successivamente per confrontare i gruppi individuati con le categorie originali.

## 3. Analisi esplorativa dei dati (EDA)

L'analisi esplorativa è stata utilizzata per esaminare la distribuzione dei dati, le caratteristiche dei permessi e la presenza di configurazioni ripetute.

È stato inizialmente calcolato il numero totale di permessi richiesti da ogni applicazione, sommando le 86 caratteristiche binarie. Le distribuzioni delle applicazioni benigne e dei malware sono state confrontate attraverso un istogramma.

<img width="1784" height="665" alt="image" src="https://github.com/user-attachments/assets/e64d13c0-7ade-4696-9064-98e54bd201a6" />

La distribuzione della variabile `Result` mostra 14.632 applicazioni benigne (49,88%) e 14.700 malware (50,12%). Le due categorie risultano quindi pressoché bilanciate.

<img width="724" height="493" alt="image" src="https://github.com/user-attachments/assets/567bdfa3-a6a9-4292-aa2b-d9196128878a" />

Successivamente, sono state calcolate le frequenze relative dei singoli permessi nelle due categorie. Sono stati selezionati i dieci permessi con la maggiore differenza assoluta tra le frequenze, rappresentandoli attraverso un grafico a barre orizzontali. Le differenze osservate indicano una diversa distribuzione dei permessi tra le categorie, senza implicare rapporti causali.

<img width="1397" height="572" alt="image" src="https://github.com/user-attachments/assets/c3f43e76-6f55-4c0c-ad69-8018a5bdd614" />

La verifica della qualità del dataset non ha rilevato valori mancanti. Sono stati individuati 21.841 record completamente duplicati, identici sia nelle caratteristiche sia nella variabile `Result`, e 21.937 profili di permessi ripetuti, indipendentemente dalla categoria.

Sono state inoltre individuate 96 configurazioni di permessi condivise da applicazioni benigne e malware, associate complessivamente a 5.624 applicazioni: 4.387 benigne e 1.237 malevole.

Considerando le configurazioni distinte, il dataset presenta 7.395 profili differenti. Di questi, 5.347 compaiono una sola volta, mentre il più frequente è condiviso da 2.009 applicazioni.

Infine, è stato calcolato il numero di permessi attivi per ciascun profilo distinto. La media è di 12,24 permessi, la mediana di 11, il minimo di 0 e il massimo di 65.

Mathur, A., Podila, L. M., Kulkarni, K., Niyaz, Q., & Javaid, A. Y. (2021). *NATICUSdroid: A malware detection framework for Android using native and custom permissions*. Journal of Information Security and Applications, 58, 102696.

UCI Machine Learning Repository. *NATICUSdroid (Android Permissions)*. https://doi.org/10.24432/C5FS64
