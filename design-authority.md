<!-- TOC -->
* [TODO cose da dettagliare](#todo-cose-da-dettagliare)
* [Design Authority](#design-authority)
  * [Gestione nuova feature/change request](#gestione-nuova-featurechange-request)
    * [Impatti architetturali](#impatti-architetturali)
    * [Processo di Design Authority](#processo-di-design-authority)
    * [Compilazione Architecture Decision Record (ADR)](#compilazione-architecture-decision-record-adr)
<!-- TOC -->

# TODO cose da dettagliare
- processo di come la design authority si incastra negli attuali processi
  - quando ingaggiare la design authority
  - documentazione richiesta
  - strategie di log
  - implementazione di integrazione con servizi esterni (log, cache, db, code, etc...) tramite facade/interfacce o pattern simili che nascondano l'implementazione e che siano switchabili facilmente



# Design Authority
## Gestione nuova feature/change request

**TODO** elenco step

### Impatti architetturali
Nel momento in cui un team deve gestire una nuova feature/change request potrebbe essere necessario effettuare tutta una serie di 
passaggi preliminari affinché:
- vi sia una storico delle decisioni architetturali e dei conseguenti tradeoff
- cliente e fornitore siano allineati sui tradeoff derivati dall'introduzione di modifiche architetturali
- utilizzo e condivisione di razionali e metriche che hanno guidato il processo di design

Di seguito viene riportata una lista di azioni che possono avere un impatto a livello architetturale, e che dunque
richiedono l'attivazione del processo di Design Authority:
- introduzione di un nuovo componente architetturale, come ad esempio:
  - front-end
  - back-end
  - coda/topic
  - microservizio
  - database
  - batch
- introduzione di nuovi endpoint su back-end/microservizi
- introduzione di nuovi elementi su database:
  - schema
  - tabelle
  - viste
  - trigger
- introduzione di nuove relazioni tra componenti architetturali
- 
### Processo di Design Authority
A fronte del rilevamento di impatti architetturali, si dovrà dunque attivare il processo di Design Authority che si occuperà
di validare e regolamentare le soluzioni individuate per soddisfare i nuovi requisiti. 

In base al tipo ti impatto architetturale sono state redatte una serie di guide 


### Compilazione Architecture Decision Record (ADR)
TODO indicare a cosa serve, perche va compilato e fare riferimento all'elenco di template ADR
con uno scope specifico
