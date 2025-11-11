# Guida per la realizzazione di diagrammi architetturali con C4 Model e Structurizr

<!-- TOC -->
* [Guida per la realizzazione di diagrammi architetturali con C4 Model e Structurizr](#guida-per-la-realizzazione-di-diagrammi-architetturali-con-c4-model-e-structurizr)
  * [Scopo del documento](#scopo-del-documento)
  * [Quando è necessario aggiornare il codice architetturale?](#quando-è-necessario-aggiornare-il-codice-architetturale)
  * [Standard architetturali e tools a supporto](#standard-architetturali-e-tools-a-supporto)
    * [C4 Model](#c4-model)
    * [Structurizr](#structurizr)
      * [Avviare Structurizr Lite in locale](#avviare-structurizr-lite-in-locale)
  * [Organizzazione del repository](#organizzazione-del-repository)
    * [Base](#base)
    * [Themes](#themes)
    * [Architecture](#architecture)
  * [Linee guida](#linee-guida)
    * [Convezioni sui file DSL](#convezioni-sui-file-dsl)
      * [Convenzioni/regole generali](#convenzioniregole-generali)
      * [Convenzioni/regole da rispettare per `software-system.dsl`](#convenzioniregole-da-rispettare-per-software-systemdsl)
      * [Convenzioni/regole da rispettare per `relationships.dsl`](#convenzioniregole-da-rispettare-per-relationshipsdsl)
        * [Esempio](#esempio)
      * [Convenzioni/regole da rispettare per `views.dsl`](#convenzioniregole-da-rispettare-per-viewsdsl)
      * [Convenzioni/regole da rispettare per `deployments.dsl`](#convenzioniregole-da-rispettare-per-deploymentsdsl)
      * [Convenzioni/regole da rispettare per `archetypes.dsl`](#convenzioniregole-da-rispettare-per-archetypesdsl)
      * [Convenzioni/regole da rispettare per `workspace.dsl`](#convenzioniregole-da-rispettare-per-workspacedsl)
      * [Convenzioni/regole da rispettare per `base.dsl`](#convenzioniregole-da-rispettare-per-basedsl)
    * [Creazione/riutilizzo di archetype](#creazioneriutilizzo-di-archetype)
    * [Convenzioni "technology"](#convenzioni-technology)
    * [Utilizzo di tag](#utilizzo-di-tag)
    * [Utilizzo di properties](#utilizzo-di-properties)
    * [Gestione temi](#gestione-temi)
    * [Scripting tramite Groovy](#scripting-tramite-groovy)
      * [Diagram finalizer](#diagram-finalizer)
  * [Architettura Structurizr Server on-premises](#architettura-structurizr-server-on-premises)
  * [Modifiche architetturali e flusso Git](#modifiche-architetturali-e-flusso-git)
  * [FAQ](#faq)
  * [Riferimenti](#riferimenti)
<!-- TOC -->

## Scopo del documento

Questa guida ha lo scopo di fornire un insieme di regole, riferimenti e buone pratiche per la creazione e la manutenzione dei diagrammi architetturali della piattaforma, adottando lo **standard C4 Model** e utilizzando **Structurizr** come strumento di modellazione.  
Il documento è rivolto a chiunque debba aggiungere o modificare i diagrammi, così da garantire coerenza con le convenzioni di progetto e con le linee guida concordate con ITAS.

---

## Quando è necessario aggiornare il codice architetturale?
È necessario aggiornare il codice architetturale nelle seguenti casistiche:
- introduzione di un nuovo componente architetturale, come ad esempio:
  - front-end
  - back-end
  - coda/topic
  - microservizio
  - database
  - batch
- introduzione di nuove relazioni tra componenti architetturali
- upgrade delle versioni di framework/librerie/runtime
- modifiche infrastrutturali, come ad esempio:
  - componente spostato da una VM a un'altra
  - modifica del numero di repliche dei POD
  - introduzione di nodi architetturali come Load Balancer, DNS, Servizi di storage, etc...

---

## Standard architetturali e tools a supporto

### C4 Model

Il **C4 Model** è un approccio semplice ma potente per descrivere e comunicare l’architettura software.  
L’idea di base è rappresentare il sistema a diversi livelli di astrazione, in modo da poter fornire il giusto grado di dettaglio a pubblici differenti: manager, architetti, sviluppatori o stakeholder non tecnici.

Il modello prevede 4 tipi di astrazione:
1. **Software System** – Livello di astrazione più alto, descrive qualcosa che porti un valore ai propri utenti, generalmente si associa a ciò che un team può sviluppare.
2. **Container** – Rappresenta un applicazione o una base dati, o in generale qualsiasi cosa possare essere deployata in modo isolato (es. un microservizio, una SPA, un DB schema, etc...).
3. **Component** – Gruppo di elementi che gestiscono una specifica funzionalità di un container, generalmente si identifica con package (java) o moduli (javascript), e dunque strettamente legato al tipo di tecnologia con cui un container viene sviluppato.
4. **Code** – Rappresenta il codice di un component, dunque si identifica in classi, interfacce, funzioni, etc... in base al linguaggio di programmazione utilizzato.

E prevede quattro livelli principali di rappresentazione:

1. **System Context Diagram** – Offre una visione d’insieme di un software system e delle sue interazioni con utenti e altri sistemi esterni.
2. **Container Diagram** – Mostra come il sistema è suddiviso in applicazioni, servizi, database o altri contenitori "deployabili" che compongono un software system.
3. **Component Diagram** – Dettaglia l’interno di un container, evidenziando i componenti che lo compongono e le relazioni tra essi.
4. **Code Diagram** – Fornisce, dove necessario, una rappresentazione a basso livello del codice o delle strutture dati coinvolte all'interno di un componente.

Oltre a questi, il C4 Model contempla anche diagrammi di supporto come il **Deployment Diagram** per descrivere l’installazione fisica o cloud delle componenti, il **Dynamic Diagram** per rappresentare flussi temporali,
e il **System Landscape Diagram** per avere una mappa completa dei sistemi dell’organizzazione.

La documentazione ufficiale è disponibile sul sito [https://c4model.com/](https://c4model.com/).

---

### Structurizr

**Structurizr** è lo strumento che adottiamo per trasformare i concetti del C4 Model in diagrammi concreti, mantenendo la filosofia del *diagrams as code*.  
A differenza di molti strumenti di modellazione grafica, Structurizr consente di descrivere
il modello architetturale utilizzando un linguaggio dichiarativo (DSL) o API per linguaggi
come Java, Groovy e Kotlin. Questo approccio consente di versionare il modello insieme al codice sorgente,
applicare modifiche in modo controllato e generare i diagrammi in maniera coerente.

Le funzionalità principali includono:
- Definizione del modello C4 e delle relative viste tramite DSL.
- Suddivisione del codice rappresentante l'architettura in file e folder differenti per miglior manutenzione dello stesso.
- Applicazione di stili e temi personalizzati (es. AWS, Azure, GCP, Kubernetes).
- Possibilità di esportare in diversi formati (PlantUML, Mermaid, DOT e altri).
- Supporto a scripting per generare o modificare dinamicamente parti del modello e per creare filtri e viste complessi.

La documentazione ufficiale e completa di esempi si trova all’indirizzo [https://docs.structurizr.com/](https://docs.structurizr.com/).

#### Avviare Structurizr Lite in locale

Per lo sviluppo in locale del codice architetturale utilizziamo **Structurizr Lite**, una versione gratuita e self-contained
di Structurizr che permette di visualizzare e modificare i diagrammi direttamente dal browser.

L'esecuzione del software avverrà tramite immagine Docker, utilizzando i seguenti comandi:

```bash
# Effettuo il pull dell'immagine di Structurizr Lite
docker pull structurizr/lite
# Avvio Structurizr Lite
docker run -it --rm -p 8080:8080 -v <architectureRepositoryPath>:/usr/local/structurizr structurizr/lite
```

Dove `architectureRepositoryPath` corrisponde al root folder del repository in cui è stato
clonato il repository contenente il codice dell'architettura, in questo modo qualsiasi modifica effettuata
alla DSL o tramite UI (vedi layout manuali) verrà sincronizzata con il proprio file system.

Dopo l’avvio, basta aprire http://localhost:8080 per accedere all’interfaccia grafica.
La guida ufficiale, completa di istruzioni e opzioni aggiuntive, è disponibile alla pagina [Structurizr Lite Quickstart](https://docs.structurizr.com/lite/quickstart).



---

## Organizzazione del repository
Il repository che contiene i diagrammi Structurizr è organizzato in modo da separare chiaramente il modello DSL,
i temi grafici, gli script di supporto e gli elementi "base" che verranno riutilizzati per descrivere l'intera architettura.

I folder principali sono i seguenti:

### Base
Folder che contiene tutti gli archetypes e gli stili comuni necessari a rappresentare l'architettura,
contiene vari subfolder divisi generalmente per tecnologia.
Ogni archetype risulta correttamente taggato affinchè sia possibile creare viste filtrate secondo le necessità.

### Themes
Qui sono contenuti i temi utilizzati per rappresentare tramite iconografia elementi architetturali, in particolare
elementi cloud come servizi AWS, Azure, GCP, etc...
Nel subfolder `base` è stato inserito un theme che contiene tutta l'iconografia e gli stili di tecnologie non presenti nei temi
di default di Structurizr (es. icone di Angular, React, etc...).

Ulteriori informazioni sui temi sono disponibili alla seguente pagina [Structurizr Themes](https://docs.structurizr.com/dsl/cookbook/themes/).

### Architecture
In questo folder è presente il codice che rappresenta l'intera architettura ITAS, dunque qui saranno descritti:
- i vari software system coinvolti, inclusi i sistemi esterni e le varie tipologie di utenti coinvolti
- i container che costituiscono i vari software system e le relative tecnologie
- le relazioni tra container e software system
- le informazioni di deployment dei vari software system/container
- tutti i metadati associati a software system e container

Per una migliore organizzazione del codice sono stati previsti al momento i seguenti subfolder:
- **aws** - include software system/container presenti su Amazon Web Services
- **onpremise** - include software system/container presenti nell'infrastruttura on premise
- **external** - include tutti i software system esterni ad ITAS (es. IVASS, ANIA, etc...)
- **gcp** - include software system/container presenti sulla Google Cloud Platform
- **mobile-app** - include software system/container che rappresentano le app mobile attualmente in utilizzo

---

## Linee guida
Per la stesura dei diagrammi tramite Structurizr e l'adesione ai principi del **C4 Model** fare sempre riferimento alle pagine ufficiali
[https://docs.structurizr.com/](https://docs.structurizr.com/) e [https://c4model.com/](https://c4model.com/).

Inoltre di seguito sono elencate le regole da rispettare:

### Convezioni sui file DSL
I file (o le tipologie di file) ammessi sono elencati nella tabella qui di seguito:

| File                  | Descrizione                                                                                                                                       |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| `software-system.dsl` | Contiene la definizione del **software system** e dei relativi **container** che lo compongono                                                    |
| `relationships.dsl`   | Descrive le **relazioni** tra il software system e i relativi container, ed eventualmente le relazioni tra essi e altri software system/container |
| `views.dsl`           | Elenco delle **viste** disponibili per il software system descritto nel file `software-system.dsl`                                                |
| `deployments.dsl`     | Contiene i vari infrastructure/deployment node che compongono un **deployment diagram**                                                           |
| `archetypes.dsl`      | Contiene gli **archetypes**                                                                                                                       |
| `styles.dsl`          | Contiene gli **stili** di specifici archetypes                                                                                                    |
| `*.container.dsl`     | File utilizzati per organizzare meglio i container nel caso in cui il relativo numero sia elevato (es. `mule`)                                    |
| `*.groovy`            | File contenti scripting in Groovy, utili per la creazione/modifica dinamica di elementi e viste                                                   |
| `workspace.dsl`       | File principale che include tutte le dichiarazioni necessarie a rappresentare l'architettura                                                      |
| `base.dsl`            | Workspace base che contiene solamente archetypes/stili/temi, necessita di essere esteso                                                           |

Il contenuto di alcuni dei file elencati nella tabella precedente dovrà dunque rispettare una serie di convenzioni e regole  
descritte nel dettaglio nelle sezioni successive. 



#### Convenzioni/regole generali
- In base a dove il software system/container è deployato, il relativo codice dovrà essere riposto all'interno di uno dei folder `aws`/`onpremise`/`external` etc...

#### Convenzioni/regole da rispettare per `software-system.dsl`
- Nel caso in cui uno o più software system collaborano strettamente tra loro o comunque hanno funzionalità legate allo stesso dominio, è possibile definirli all'interno di un unico file `software-system.dsl` (es. `sinistri`)
- Tale file deve essere inserito allo stesso livello dei relativi file `relationships.dsl` e `views.dsl`
- Evitare di elencare elementi troppo specifici come components e code, se non per casi in cui è necessario
descrivere in modo approfondito alcune feature particolarmente core e critiche
- I software system/container devono essere descritti utilizzando degli opportuni archetypes, evitando dunque
di fare affidamento alle keywork `container` o `softwareSystem`, se non in casi ben specifici
- I software system/container devono essere descritti in modo conforme alle specifiche del **C4 Model**

#### Convenzioni/regole da rispettare per `relationships.dsl`
- Elencare le relazioni tra utenti e software system descritti nel file `software-system.dsl`
- Elencare le relazioni software system descritti nel file `software-system.dsl` ed eventuali altri software system non ivi descritti
- Elencare tutte le relazioni tra i container del software system rappresentato
- Elencare le relazioni tra i container del software system rappresentato e container relativi a eventuali altri software system non ivi descritti
- Nel rappresentare relazioni verso software system/container esterni a quelli dichiarati `software-system.dsl`
  includere solamente il verso software `system/container "interno" >> software system/container "esterno"`
- Nel caso vi siano relazioni a doppio senso con sistemi di integrazione quali gestori di code/topic,
  tali relazioni possono essere espresse in questo file con entrambi i versi
- Qualora vi sia un numero eccessivo di relazioni è preferibile utilizzare
  la generazione dinamica delle relazioni tramite il tag `!elements` o tramite scripting
- Tale file deve essere inserito allo stesso livello dei relativi file `software-system.dsl` e `views.dsl`
- Le relazioni devono essere descritte utilizzando degli opportuni archetypes, evitando dunque
  di fare affidamento alle keywork `->'`, se non in casi ben specifici
- Le relazioni devono essere descritte in modo conforme alle specifiche del **C4 Model**

##### Esempio
```structurizrdsl
// File `software-system.dsl`
user = person "User"
systemA = softwareSystem "System A" {
  containerA = container "Container A"
  containerB = container "Container B"
}

// File `relationships.dsl`
user -> systemA ...
systemA.containerA -> systemA.containerB ...
  
// Relazioni con sistemi esterni
systemA -> externalSystem ...
systemA.containerA -> systemB.container1 ...

// Supponendo che:
// - tutti i servizi esterni comunichino con "systemA"
// - tutti i servizi esterni hanno il tag "external"
// Generiamo le relazioni da tali sistemi verso "systemA"
!elements "element.tag==external" {
  this -> systemA ...
}

// Nel caso di relazioni con sistemi di integrazioni, come ad esempio code, va bene includere le 
// relazioni in entrambi i versi nel file delle relazioni del "systemA"
systemA.containerA -> awsSQS.coda1 ...
awsSQS.coda2 -> systemA.containerA ...

// Relazione da non includere qui, ma bensi nel file delle relazioni relative al sistema "systemB"
systemB.container2 -> systemA.containerA ...
```

#### Convenzioni/regole da rispettare per `views.dsl`
- Includere le viste di default di `systemContext` e `container` per i software system dichiarati nel file `software-system.dsl`
- Per i container diagram con 20 o meno container preferire sempre il layout automatico configurandolo con direzione `top-down` (valutare caso per caso se differenti tipi di layout automatico possano risultare più adatti)
- Per i container diagram con più di 20 container preferire sempre il layout manuale, avendo cura poi di formattarlo tramite Structurizr UI
- Non definire alcuna vista (comprese quelle di default) se i software system descritti
  nel file `software-system.dsl` non contengono container
- Evitare di inserire viste che rappresentino component o code diagrams, se non per casi in cui è necessario
descrivere in modo approfondito alcune feature particolarmente core e critiche
- Le seguenti `view` che descrivono diagrammi "globali" devono essere contenute esclusivamente nel file `architecture/views.dsl`:
  - `deployment`
  - `systemLandscape`
  - `container` che però includono **tutti i container** definiti nel workspace
- Tale file deve essere inserito allo stesso livello dei relativi file `software-system.dsl` e `relationships.dsl`

#### Convenzioni/regole da rispettare per `deployments.dsl`
- Devono essere sempre referenziati software system e relativi container, in modo da poter
eventualmente creare delle viste che mostrino solamente gli uni piuttosto che gli altri in base 
al livello di dettaglio che si vuole
- Nel momento in cui si vogliano raggruppare più elementi insieme, come ad esempio in una VPC, utilizzare i `deploymentNode`
- Nel caso di software system/container deployati su una **VM**, è obbligatorio definire la seguente gerarchia di `deploymentNode`:
  - Macchina fisica sulla quale si trova la VM
  - Sistema operativo della VM
  - Eventuale application/web server
  - software system/container 
- Qualora vi siano delle connessioni tra vari `deploymentNode` oltre a quelle descritte dalle relationships tra i
vari container, sarà necessario definire delle ulteriori relationships in tale file (es. connessioni VPN tra ambienti onpremise-cloud)
- È possibile creare dei file di deployments separati nel caso in cui vi siano ambienti completamente separati tra loro (es. sistemi su AWS/Azure/GCP)

[//]: # (- **TODO questa parte potrebbe cambiare, perche il file deployments attuale è gia parecchio grande**)

#### Convenzioni/regole da rispettare per `archetypes.dsl`
- Ogni file di questo tipo deve contenere esclusivamente archetypes che appartengano allo stesso "dominio tecnologico" (es. front-end, back-end, database, etc...)
- Questi file devono essere necessariamente archiviati sotto il folder `base/<dominio-tecnologico>`
- Tale file deve essere inserito allo stesso livello del relativo file `styles.dsl` (qualora presente)

#### Convenzioni/regole da rispettare per `workspace.dsl`
- Deve essere privo di informazioni circa l'architettura, bensì deve solamente includere i seguenti file:
  - DSL per software system/container/component/code
  - DSL per relazioni
  - DSL per deployments
  - DSL per viste
  - Script .groovy

#### Convenzioni/regole da rispettare per `base.dsl`
- Deve essere privo di informazioni circa l'architettura, bensì deve solamente includere:
  - archetypes
  - stili
  - temi

### Creazione/riutilizzo di archetype
Gli archetype permettono di definire strutture comuni (ad esempio un microservizio base) da riutilizzare in più punti del modello, garantendo uniformità.
- Ciascun archetype che descrive un **container** deve avere contenere obbligatoriamente (al netto di casi speciali) i seguenti attributi:
    - `technology` nel formato descritto in [convenzioni technology](#convenzioni-technology) (es. `Typescript/Angular`)
    - `tags` (in lowercase):
        - tag che indica il "dominio tecnologico" (es. `front-end`)
        - tag che indica il framework/tecnologia (es. `angular`)
        - eventuali tag per il tema (es. tema AWS)
        - eventuali tag per il cloud di appartenenza (es. `aws`)
- Ciascun archetype che descrive una **relazione** deve avere contenere obbligatoriamente (al netto di casi speciali) i seguenti attributi:
    - `technology` nel formato descritto in [convenzioni technology](#convenzioni-technology) (es. `JSON/HTTP`)
    - `tags` (in lowercase):
        - tag che indica il protocollo (es. `http`)
        - eventuale tag che indica il formato dati (es. `json`)
        - tag per eventuali temi (es. tema AWS)
- Nella definizione dell'architettura utilizzare quanto più possibile degli archetypes ben definiti
ed evitare di creare dei generici elementi se non in casi specifici (vedi informazioni mancanti o bassa rilevanza architetturale)

### Convenzioni "technology"
La proprietà `technology` seguire le seguenti convenzioni:
- deve essere sempre definita per i seguenti elementi:
  - container
  - relationship
  - deploymentNode
- nella definizione di `deploymentNode`, deve indicare il software/tecnologia/sistema operativo, alcuni esempi sono:
  - `Linux` nel caso di VM con sistema operativo Linux
  - `Tomcat`
  - `JBoss`
  - `Oracle`
- nella definizione di `container` che rappresentano del software sviluppato bisogna rispettare il formato `[Linguaggio/]Framework`, ad esempio:
  - `Typescript/Angular`
  - `Typescript/React`
  - `Java/Spring`
- nella definizione delle relazioni bisogna rispettare il formato `[Formato dati/]Protocollo`, ad esempio:
    - `JSON/HTTP`
    - `XML/HTTPS`
    - `JSON/Websocket`
    - `SQL/TCP`
    - `VPN`
- nel caso indichi un sistema operativo, cercare di essere quanto più specifici possibile 
(es. nel caso di Linux indicare la distro anziché un generico °linux°)
- non deve **mai includere** la versione di un software/runtime/framework/SO/etc... in quanto questa deve 
essere indicata tramite `properties`

### Utilizzo di tag
Ogni elemento del modello (sistemi, container, componenti) deve essere corredato di tag obbligatori.
Di seguito è riportata una tabella in cui vengono indicati i tag da utilizzare per ciascun elemento.

| Tag / Tipologia tag | Software System | Container | Deployment node | Infrastructure node | Descrizione                                                                          | Formato                             | Esempio                                                |
|---------------------|:---------------:|:---------:|:---------------:|:-------------------:|--------------------------------------------------------------------------------------|-------------------------------------|--------------------------------------------------------|
| technology          |        -        |     X     |        -        |          -          | Riporta il valore della proprietà `technology` per migliorare la ricerca tramite tag | alfanumerico lowercase              | `angular` (da technology `Typescript/Angular`)         |
| dominio tecnologico |        -        |     X     |        -        |          -          | Dominio tecnologico di appartenenza                                                  | alfanumerico lowercase              | `front-end` - `back-end` - `microservice` - `database` |
| on premise          |        -        |     -     |        X        |          X          | Indica se il nodo di riferimento è deployato su ambiente on premise                  | unico valore accettato: `onpremise` | `onpremise`                                            |
| cloud provider      |        -        |     X     |        X        |          X          | Indica il cloud provider di appartenenza                                             | alfanumerico lowercase              | `aws` - `azure`                                        |
| servizio cloud      |        -        |     -     |        X        |          X          | Indica il servizio cloud che rappresenta il nodo                                     | alfanumerico lowercase              | `AKS` - `S3`                                           |
| risorsa kubernetes  |        -        |     -     |        X        |          X          | Indica la tipologia di risorsa kubernetes utilizzata come nodo                       | alfanumerico lowercase              | `pod` - `replica-set`                                  |

Il simbolismo utilizzato è il seguente:
- `X` obbligatorio nell'elemento di riferimento
- `O` opzionale nell'elemento di riferimento

### Utilizzo di properties
In aggiunta ai `tag`, alcuni elementi devono anche prevedere la presenza di alcune properties per aggiungere maggiore livello di 
dettaglio all'architettura e permettere quindi l'utilizzo di filtri più complessi per la creazione di viste specifiche.
Alcune properties possono avere un significato generico, come ad esempio l'area di appartenenza piuttosto che gli owner 
di uno specifico sistema, altre invece vanno a dettagliare una specifica tecnologia, indicando ad esempio il numero di versione 
di un framework/linguaggio. 

Di seguito una tabella di quelle che sono le properties ammesse, ricavate direttamente dai constraint applicati dallo script 
di controllo: 

[//]: # (**TODO indicare script una volta implementato**)

| Property                   | Software System | Container | Deployment node | Infrastructure node | Descrizione                                 | Formato                         | Esempio                      |
|----------------------------|:---------------:|:---------:|:---------------:|:-------------------:|---------------------------------------------|---------------------------------|------------------------------|
| `activemqVersion`          |                 |           |                 |                     |                                             |                                 |                              |
| `alfrescoVersion`          |                 |           |                 |                     |                                             |                                 |                              |
| `androidSupportedVersions` |                 |           |                 |                     |                                             |                                 |                              |
| `angularVersion`           |                 |           |                 |                     |                                             |                                 |                              |
| `area`                     |        X        |     O     |        -        |          -          | Indica l'area di appartenenza dell'elemento | alfanumerico lowercase          | `portafoglio-danni`          |
| `cassandraVersion`         |                 |           |                 |                     |                                             |                                 |                              |
| `delphiVersion`            |                 |           |                 |                     |                                             |                                 |                              |
| `deployment`               |                 |           |                 |                     |                                             |                                 |                              |
| `deprecated`               |                 |           |                 |                     |                                             |                                 |                              |
| `dotNetVersion`            |                 |           |                 |                     |                                             |                                 |                              |
| `flutterVersion`           |                 |           |                 |                     |                                             |                                 |                              |
| `invokedByMule`            |                 |           |                 |                     |                                             |                                 |                              |
| `invokesMule`              |                 |           |                 |                     |                                             |                                 |                              |
| `iosSupportedVersions`     |                 |           |                 |                     |                                             |                                 |                              |
| `javaVersion`              |                 |           |                 |                     |                                             |                                 |                              |
| `jbossVersion`             |                 |           |                 |                     |                                             |                                 |                              |
| `lambdaRuntime`            |                 |           |                 |                     |                                             |                                 |                              |
| `lambdaRuntimeVersion`     |                 |           |                 |                     |                                             |                                 |                              |
| `liferayVersion`           |                 |           |                 |                     |                                             |                                 |                              |
| `linuxDistro`              |                 |           |                 |                     |                                             |                                 |                              |
| `opconVersion`             |                 |           |                 |                     |                                             |                                 |                              |
| `oracleVersion`            |                 |           |                 |                     |                                             |                                 |                              |
| `owners`                   |        X        |     O     |        -        |          -          | Indica gli owner dell'elemento              | alfanumerico lowercase (lista)  | `fincons` o `fincons,reply`  |
| `pentahoVersion`           |                 |           |                 |                     |                                             |                                 |                              |
| `postgresqlVersion`        |                 |           |                 |                     |                                             |                                 |                              |
| `reactVersion`             |                 |           |                 |                     |                                             |                                 |                              |
| `solrVersion`              |                 |           |                 |                     |                                             |                                 |                              |
| `springVersion`            |                 |           |                 |                     |                                             |                                 |                              |
| `tomcatVersion`            |                 |           |                 |                     |                                             |                                 |                              |
| `versioning`               |                 |           |                 |                     |                                             |                                 |                              |
| `visualbasicVersion`       |                 |           |                 |                     |                                             |                                 |                              |
| `websphereVersion`         |                 |           |                 |                     |                                             |                                 |                              |
| `windowsVersion`           |                 |           |                 |                     |                                             |                                 |                              |

Il simbolismo utilizzato è il seguente:
- `X` obbligatorio nell'elemento di riferimento
- `O` opzionale nell'elemento di riferimento


[//]: # (### Rappresentare un microservizio)

[//]: # (TODO Lotto 2/3)

[//]: # ()
[//]: # (### Rappresentare un database)

[//]: # (TODO Lotto 2/3)

[//]: # ()
[//]: # (### Integrazioni con code asincrone)

[//]: # (TODO Lotto 2/3)

[//]: # ()
[//]: # (### Deployment diagram)

[//]: # (TODO Lotto 2/3)

### Gestione temi
Nel progetto sono disponibili temi preconfigurati per ambienti AWS, Azure, GCP e Kubernetes. I temi si trovano nel folder `themes` e sono inclusi nel file `base.dsl`.
Qualora fosse necessario aggiungere dei nuovi temi sarà necessario scaricare sia le immagini che il relativo file `theme.json`
nel folder `themes/<new theme>`, mantenendo una struttura flat (immagini e file json devono essere allo stesso livello senza ulteriori subfolder).

Qualora si volessero scaricare dei temi dal sito ufficiale di Structurizr è disponibile lo script Powershell [utility/download-structurizr-theme.ps1](../../diagrams/utility/download-structurizr-theme.ps1) per 
scaricarli in automatico. 

Per lanciarlo eseguire il seguente comando:
```shell
download-structurizr-theme.ps1 -ThemeUrl "<theme-url>/theme.json" -DestinationFolder "<path-to-repository>/themes/<new-theme>"
```

Ad esempio, se si volesse scaricare il tema `kubernetes-v0.3`, il comando da lanciare è il seguente:
```shell
download-structurizr-theme.ps1 -ThemeUrl "https://static.structurizr.com/themes/kubernetes-v0.3/theme.json" -DestinationFolder "<path-to-repository>/themes/kubernetes-v0.3"
```

### Scripting tramite Groovy
Structurizr supporta script Groovy per generare parti di modello in maniera dinamica, ad esempio popolando automaticamente i componenti a partire dal codice sorgente.
In questo progetto sono stati introdotti una serie di script per introdurre alcune automazioni.

#### [Diagram finalizer](../../diagrams/diagram-finalizer.groovy)
Effettua le seguenti operazioni automatiche:
- trasforma la properties `owner` in tag a se stanti (es. `reply,fincons` diventano due tag separati) 
- eredita automaticamente la property `area` e `owners` qualora il software system di un container le contenga
- eredita in modo ricorsivo il tag `aws` in deployment node innestati, evitando dunque di definirlo multiple volte nel caso di gerarchie complesse di nodi

---

## Architettura Structurizr Server on-premises

[//]: # (**TODO declinare ulteriormente le modalita di installazione**)

In ambiente di produzione, il cliente adotta Structurizr Server installato on-premises.
Questa modalità offre funzionalità aggiuntive rispetto alla versione Lite, tra cui il supporto multi-utente, il salvataggio centralizzato dei workspace, 
il controllo degli accessi, l’integrazione con sistemi di autenticazione aziendale e la gestione delle versioni.

L’architettura tipica prevede:
- Un’istanza del server Structurizr ospitata all’interno della rete aziendale.
- Database dedicato per il salvataggio dei modelli e delle configurazioni.
- Accesso sicuro tramite HTTPS e autenticazione centralizzata.
- Integrazione con pipeline CI/CD per aggiornare i diagrammi automaticamente.

Le feature aggiuntive della versione on-premises sono descritte nella sezione “Products” della documentazione ufficiale: https://docs.structurizr.com/products.

---

## Modifiche architetturali e flusso Git
Ogni qualvolta dovessero essere necessarie delle modifiche a livello architetturale, sarà necessario 
richiedere una revisione delle stesse utilizzando il repository `<todo-repository>` assieme a Git.
Ogni modifica deve passare attraverso una merge request e un processo ibrido di validazione manuale e automatizzata della stessa.

Il flusso di lavoro prevede dunque i seguenti step, indicati anche nella figura successiva:

![Git flow per modifiche diagrammi architetturali](.\assets\diagram_git_flow.png "Git flow diagrammi architetturali")

1. Il team responsabile del design delle nuove specifiche architetturali dovra creare un nuovo branch nel formato `feature/<Work-ID>-<title>`
2. Il team applica tutte le modifiche/aggiunte al codice DSL
3. Il team crea una merge request del branch creato al punto 1 verso il branch `main`
4. Una pipeline correttamente configurata sull'evento di creazione di merge request si occuperà di eseguire una serie di script contenuti nel repository stesso per valutare l'adesione alle linee guida architetturali (es. inserimento dei tag, gestione delle properties, etc...). Qualora questo step dovesse fallire, il team dovrà risolvere gli errori indicati fintanto che la pipeline non terminerà con successo.
5. La merge request verrà sottoposta a un check manuale da parte della design authority, che validerà le modifiche e confermerà il merge delle stesse
6. A merge avvenuto, una pipeline si occuperà di aggiornare il codice DSL presente sul server Structurizr

---

## FAQ

---

## Riferimenti
C4 Model: https://c4model.com/

Structurizr Documentation: https://docs.structurizr.com/

---

[//]: # (## TODO Integrazioni con AI)

[//]: # (- demo da poter fare)

[//]: # (  - estrarre una tabella con tutte le versioni di framework/librerie &#40;tramite script ad hoc&#41; e chiedere ad AI e chiedere quali sono i componenti potenzialmente obsoleti)

[//]: # (  - estrarre l'elenco di tutte le relazioni &#40;tramite script ad hoc&#41; e chiedere ad AI e quali sono i componenti connessi al componente X)
