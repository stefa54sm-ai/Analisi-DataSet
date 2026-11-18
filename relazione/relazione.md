# Analisi dei permessi delle applicazioni Android per il rilevamento di malware

## 1. Introduzione

Il presente progetto si propone di analizzare un insieme di dati relativo alle applicazioni Android, con particolare attenzione alla distinzione tra applicazioni legittime (*benign*) e applicazioni malevole (*malware*).

L'obiettivo è studiare le caratteristiche delle applicazioni attraverso tecniche di analisi dei dati e di apprendimento automatico, cercando di individuare eventuali somiglianze, differenze e raggruppamenti tra le osservazioni.

Il lavoro si concentra in particolare sui permessi richiesti dalle applicazioni Android, ossia le autorizzazioni necessarie per accedere a determinate funzionalità del dispositivo o alle informazioni dell'utente. L'insieme dei permessi richiesti può infatti fornire indicazioni sulle caratteristiche di un'applicazione e contribuire all'individuazione di comportamenti potenzialmente sospetti.

Un aspetto centrale del progetto consiste nell'applicazione di un algoritmo di analisi dei dati affrontato durante il corso di Principi e Modelli della Percezione. Attraverso tale algoritmo si intende verificare se sia possibile individuare strutture e gruppi di applicazioni sulla base delle caratteristiche condivise.

L'analisi computazionale sarà affiancata dalla rappresentazione visiva dei dati. In particolare, verranno presi in considerazione i principi della Gestalt, come prossimità, somiglianza e raggruppamento, per osservare come la disposizione e la codifica visiva delle informazioni possano facilitare il riconoscimento di schemi e relazioni.

Il progetto mira quindi a collegare l'organizzazione matematica dei dati con i meccanismi attraverso cui l'essere umano percepisce e interpreta le informazioni visive.

## 2. Descrizione del dataset

Per lo svolgimento del progetto è stato selezionato il dataset **NATICUSdroid (Android Permissions)**, disponibile presso l'UCI Machine Learning Repository.

Il dataset raccoglie informazioni relative a 29.333 applicazioni Android, distribuite tra applicazioni legittime e applicazioni malevole. I dati sono stati raccolti da applicazioni rilasciate nel periodo compreso tra il 2010 e il 2019.

Ogni osservazione rappresenta una singola applicazione Android, mentre le caratteristiche (*features*) descrivono la presenza o l'assenza di specifici permessi richiesti dall'applicazione.

Il dataset presenta 86 caratteristiche, rappresentate attraverso variabili numeriche intere con codifica binaria:

- **1:**  il permesso è presente tra quelli richiesti dall'applicazione;
- **0:**  il permesso non è presente tra quelli richiesti dall'applicazione.

Oltre alle caratteristiche, il dataset contiene una variabile target denominata `Result`, che identifica la categoria dell'applicazione, distinguendo tra applicazioni legittime e malevole.

La struttura dei dati consente di rappresentare ogni applicazione come un vettore composto da valori binari. È quindi possibile confrontare le applicazioni in base ai permessi condivisi e alle differenze tra le rispettive caratteristiche.

Questa tipologia di dati risulta interessante per l'applicazione di tecniche di clustering e classificazione. In particolare, sarà possibile analizzare se applicazioni con permessi simili tendano a presentare anche la stessa categoria di appartenenza.

La variabile `Result` verrà considerata durante l'analisi esplorativa e nella valutazione dei risultati. Nel caso delle tecniche di clustering non supervisionato, invece, non verrà utilizzata come caratteristica in ingresso all'algoritmo, ma servirà successivamente per confrontare i gruppi individuati con le categorie originali.

**Fonte del dataset:**

Mathur, A. (2022). *NATICUSdroid (Android Permissions)*. UCI Machine Learning Repository.

https://doi.org/10.24432/C5FS64
