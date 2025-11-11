# TODO Idee improvement architettura da proporre
#### Eliminare active MQ:
- versione obsoleta
- non riesce a gestire corrattamente il carico
- non ha DLQ
- necessaria macchina ad hoc

soluzione: usare servizio Azure

valutare:
- connessione tra onpremise e azure
- escludere che vi siano utilizzi di ActiveMQ da AWS

---

#### Usare ansible per far scalare la macchine di AJI


