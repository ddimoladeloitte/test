<!-- TOC -->
* [Gestione progetti con Azure DevOps e Git](#gestione-progetti-con-azure-devops-e-git)
  * [Introduzione](#introduzione)
  * [Creazione di un nuovo progetto in Azure DevOps](#creazione-di-un-nuovo-progetto-in-azure-devops)
    * [Quando creare un nuovo progetto](#quando-creare-un-nuovo-progetto)
    * [Naming convention](#naming-convention)
    * [Sicurezza e accessi](#sicurezza-e-accessi)
  * [Struttura dei repository Git](#struttura-dei-repository-git)
    * [Naming convention](#naming-convention-1)
    * [Semantic versioning](#semantic-versioning)
    * [Tags](#tags)
    * [Branching Strategy e Politiche di Protezione](#branching-strategy-e-politiche-di-protezione)
    * [Merge Strategy](#merge-strategy)
      * [Revisione Merge/Pull request](#revisione-mergepull-request)
      * [Squash merge](#squash-merge)
    * [Monorepository vs Multi-repo](#monorepository-vs-multi-repo)
  * [TODO Gestione dell’Infrastruttura e IaC](#todo-gestione-dellinfrastruttura-e-iac)
  * [TODO Governance e Best Practice trasversali](#todo-governance-e-best-practice-trasversali)
  * [TODO Esempi pratici](#todo-esempi-pratici)
  * [TODO Integrazioni con AI](#todo-integrazioni-con-ai)
<!-- TOC -->

---

# Gestione progetti con Azure DevOps e Git

---

## Introduzione

Questa guida fornisce uno standard aziendale per la creazione e configurazione di nuovi progetti in Azure DevOps. L'obiettivo è garantire coerenza, sicurezza e una gestione efficace dei repository Git, integrando in modo naturale gli strumenti di project management e DevOps.

**Assunzioni di base:**

* Ogni progetto sarà gestito da team indipendenti.
* Ogni team avrà autonomia sul proprio ciclo di vita, ma dovrà rispettare standard comuni.
* La guida si focalizza sull'utilizzo di Azure Repos, Boards e Pipelines.

---

## Creazione di un nuovo progetto in Azure DevOps

La creazione di un nuovo progetto Azure DevOps dovrà rispettare una serie di requisiti per avere un maggiore controllo e segregazione 
di informazioni relative a specifici ambiti/progettualità. 

### Quando creare un nuovo progetto

Secondo le linee guida di Microsoft, la miglior prassi e quella di creare un unico progetto
anche nel caso in cui vi siano centinaia di applicazioni in esso, poiché questo approccio
garantisce una migliore connessione tra le applicazioni stesse ([https://learn.microsoft.com/en-us/azure/devops/organizations/projects/about-projects?view=azure-devops](https://learn.microsoft.com/en-us/azure/devops/organizations/projects/about-projects?view=azure-devops)).

Tuttavia, ci sono dei casi in cui e consigliato creare diversi progetti, soprattutto in ambito Enterprise.
In particolare, qualora l'organizzazione dovesse avere una delle seguenti necessità, bisognerà
optare per la creazione di un nuovo progetto in Azure DevOps:

- [ ] Introduzione di nuove funzionalità legate a domini non ancora gestiti nell'attuale architettura
- [ ] Gestire l’accesso alle informazioni contenute all’interno di un progetto per gruppi specifici
- [ ] Supportare processi personalizzati di tracciamento del lavoro per specifiche unità di business all’interno della tua organizzazione
- [ ] Coesistenza di unità di business e/o tecniche completamente separate che dispongono di proprie policy amministrative e propri amministratori

Nel caso in cui si volessero aggiungere funzionalità a domini applicativi già presenti, o qualora non vi fosse una particolare
necessità di segregazione delle informazioni a causa della presenza di team/fornitori diversi, si potrà lavorare
sugli applicativi gia esistenti, o qualora necessario, creare dei nuovi applicativi all'interno dello stesso progetto DevOps.

### Naming convention
Rispettare i seguenti requisiti per quanto riguarda la **naming convention** del progetto:
  * Utilizzo di soli caratteri alfanumerici
  * Assenza di spazi, nel caso fosse necessario separare parole utilizzare il carattere `-`

### Sicurezza e accessi
Gli accessi al progetto dovranno essere garantiti con l'approccio "least privilege", avendo
cura dunque di escludere qualsiasi utente o gruppo non abbia necessita di accedervi.

Si consiglia sempre l'utilizzo dei gruppi, opportunamente divisi, in quanto piu scalabile rispetto ad una
gestione per singolo utente.

---

## TODO Gestione dell’Infrastruttura e IaC

* Creare repository separati per Infrastructure as Code (`iac-<progetto>`).
* Usare standard come Terraform, Bicep o ARM template.
* Versionare i file IaC nello stesso modo del codice applicativo.
* Automatizzare provisioning e deploy con pipeline dedicate.

---

## TODO Governance e Best Practice trasversali

* Creare una **checklist** per la creazione di un nuovo progetto:

  * Naming corretto.
  * Area path e iteration configurati.
  * Repo inizializzato con README e CONTRIBUTING.
  * Branch policy impostate.
  * Pipeline CI attiva.
* Definire convenzioni uniformi per nomi di Project, Repo, Branch e Pipeline.
* Revisionare permessi e policies periodicamente.
* Monitorare metriche tramite [Azure DevOps Analytics](https://learn.microsoft.com/en-us/azure/devops/report/powerbi/overview).

---

## TODO Esempi pratici

**Progetto: Payments**

* Repo: `payments-backend`, `payments-frontend`, `payments-iac`.
* Branch: `main`, `develop`, `feature/123-add-api`.
* Pipeline YAML:

  * `ci-build.yml` con build, test e artifact.
  * `cd-release.yml` per Dev → Test → Prod.
* Board: area path per ogni team (backend, frontend).


