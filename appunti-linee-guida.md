# Linee guida

## Lotto 1
- Metodologia di creazione dei diagrammi
- Flusso di versionamento e revisione tramite Git, monorepo vs multi-repo e integrazione con project management
- Introduzione nuovo componente di back-end: misurazione impatti e posizionamento 
- Adozione di una libreria/framework esterna

## Lotto 2
- template documentazione progetto (indicare sostanzialmente quando si propone un nuovo progetto cosa si deve fare e cosa si deve compilare)
- Logging/tracing
- cache (e monitoraggio eviction/cache miss)
- Batch: best practice
- Microservizi: granularity integrators/disintegrators
- Portabilità applicazioni: best practice (es. definizione dei puntamenti tramite environment, o cmq pratiche per rendere 
indipendente il codice applicativo dall'ambiente in cui viene deployato)
esempi pratici di cosa fare e cosa non fare

- Gestione librerie interne
  - Semantic versioning e backward-compatibility
  
## Lotto 3
- Migrazione TFS -> GIT
- Adozione di un software/tool
- Integrazione del processo di test automation
- Definizione delle pipeline di build e rilascio
- Definizione della modalita' di rilascio con eventuali reiterazioni per la valutazione degli impatti dei componenti
- una RACI per i ruoli in tutto il processo di design authority
- una RACI per i ruoli in tutto il processo di design authority
---

## Appunti vari

## linee guida da stilare
- Design authority
- linee guida applicative:
  - utilizzare gli ADR per le scelte di libvreire/freamwoekr/linguaggi utlizzati dai vari fornitori
  - gestione dei componenti FE
  - creare una tabella globale e consultabile degli orari dei batch, questa servira per una linea guida sulla gestione dei batch
  - gestione degli errori per ogni attivita nuova:
    - es. se devo lavorare all'incasso, devono dirci come gestire gli errori
    - capire in base alla criticita dell'attivita che come gestire l'errore
- linee guida su come creare la documentazione (riccardo ha chiesto che ci siano degli standard per renderla fruibile)
- implementazione di integrazione con servizi esterni (log, cache, db, code, etc...) tramite facade/interfacce o pattern simili che nascondano l'implementazione e che siano switchabili facilmente

- linee guida lato cloud ma anche onpremise:
  - gestione del tag
  - monitoraggio
  - logging (ora loggano su file, pensare di **passare direttamente a cloud**)
  - tracing
- processo di come la design authority si incastra negli attuali processi
  - quando ingaggiare la design authority
  - documentazione richiesta

- utilizzo di convenzioni per il branching
  - inserire il riferimento al work item
  - nella merge request inserire il riferimento al work item

#### sviluppo e rilascio feature
parte lo svillpoo ci sono n settimane e disposizione
la fase di test dura 8 giorni, include:
merge su branch test
creazione pacchetti
conseegna pacchetti
installazione
test installazione
UAT
eventuali ricicli

poi 2 giorni di stress test con jmeter

1 settimana di prepord per installazione pacchetti e verifiche

poi si parte con il rilascio in prod

---

#### che linee guida si aspetta riccardo
- quando andare su microservizi oppure no
- usare sempre git
- includi i test automatici
- danilo e corrado hanno proposto il file postman per i servizi esposti
- problema dei rilasci che richiedono 2 mesi, lato business se ne vedono invece 6
- log
- 
