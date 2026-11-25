# Analisi dei permessi delle applicazioni Android per il rilevamento di malware

Il progetto analizza un dataset di applicazioni Android con l'obiettivo di studiare le differenze tra applicazioni legittime (benign) e malevole (malware).

L'analisi utilizza gli 86 permessi richiesti dalle applicazioni come caratteristiche (features) per individuare gruppi di applicazioni con configurazioni simili attraverso algoritmi di clustering. I gruppi ottenuti verranno successivamente confrontati con le categorie originali del dataset.

I risultati saranno inoltre rappresentati visivamente, facendo riferimento ad alcuni principi della Gestalt, in particolare quelli di prossimità e somiglianza.

## 1. Descrizione del dataset

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

## 2. Explorative Data Analysis (EDA)

L'analisi esplorativa è stata utilizzata per esaminare la distribuzione dei dati, le caratteristiche dei permessi e la presenza di configurazioni ripetute.

Inizialmente, è stato calcolato il numero totale di permessi richiesti da ciascuna applicazione, sommando i valori delle 86 caratteristiche binarie. Il confronto tra le applicazioni benigne e i malware è rappresentato attraverso un grafico della distribuzione cumulativa empirica (ECDF), che permette di osservare la percentuale di applicazioni che richiede un determinato numero di permessi o un numero inferiore.

<img width="1784" height="665" alt="image" src="https://github.com/user-attachments/assets/e64d13c0-7ade-4696-9064-98e54bd201a6" />

Il dataset comprende 14.632 applicazioni benigne (49,88%) e 14.700 malware (50,12%). Le due categorie risultano quindi pressoché bilanciate, senza una prevalenza significativa di una classe rispetto all'altra.

<img width="724" height="493" alt="image" src="https://github.com/user-attachments/assets/567bdfa3-a6a9-4292-aa2b-d9196128878a" />

Successivamente, sono state calcolate le frequenze dei singoli permessi nelle due categorie. Sono stati selezionati i dieci permessi con la maggiore differenza assoluta tra le rispettive frequenze, rappresentandoli attraverso un grafico a barre orizzontali. Tra le differenze più evidenti emerge il permesso READ_PHONE_STATE, presente in circa il 94% dei malware e nel 23% delle applicazioni benigne. Si osservano differenze rilevanti anche per RECEIVE_BOOT_COMPLETED e per i permessi relativi alla localizzazione.

<img width="1397" height="572" alt="image" src="https://github.com/user-attachments/assets/c3f43e76-6f55-4c0c-ad69-8018a5bdd614" />

La verifica della qualità dei dati non ha rilevato valori mancanti e ha confermato che le 86 caratteristiche rispettano la codifica binaria prevista (0 e 1). Sono stati individuati 21.841 record completamente duplicati e 21.937 applicazioni oltre la prima che condividono un profilo di permessi già presente nel dataset. In particolare, su 29.332 applicazioni sono stati individuati 7.395 profili di permessi distinti.

Un'ulteriore analisi ha individuato 96 profili di permessi associati a entrambe le categorie, per un totale di 5.624 applicazioni: 4.387 benigne e 1.237 malevole.

Questo risultato evidenzia un limite delle caratteristiche disponibili: applicazioni appartenenti a categorie differenti possono presentare la stessa configurazione di permessi. Di conseguenza, i soli permessi richiesti non sono sempre sufficienti per distinguere un'applicazione malevola da una legittima.

Questa osservazione è rilevante per le successive analisi di clustering, poiché applicazioni con lo stesso profilo occupano la stessa posizione nello spazio delle caratteristiche, pur appartenendo a classi differenti.

## Bibliografia
Mathur, A., Podila, L. M., Kulkarni, K., Niyaz, Q., & Javaid, A. Y. (2021). NATICUSdroid: A malware detection framework for Android using native and custom permissions. Journal of Information Security and Applications, 58, 102696.

UCI Machine Learning Repository. NATICUSdroid (Android Permissions). https://doi.org/10.24432/C5FS64
