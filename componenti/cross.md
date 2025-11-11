<!-- TOC -->
* [Versionamento (TODO spostare da un'altra parte)](#versionamento-todo-spostare-da-unaltra-parte)
  * [Update major](#update-major)
  * [Update minor](#update-minor)
  * [Update bug fix](#update-bug-fix)
  * [Gestione dipendenze](#gestione-dipendenze)
<!-- TOC -->

# Versionamento (TODO spostare da un'altra parte)
## Update major
- TODO

## Update minor
- Prevedere l'update delle versioni bugfix di tutte le librerie coinvolte in un progetto almeno 1 volta ogni 4 mesi


## Update bug fix
- Prevedere l'update delle versioni bugfix di tutte le librerie coinvolte in un progetto almeno 1 volta ogni 2 mesi

## Gestione dipendenze
- Utilizzare sempre le versioni esatte di librerie/framework anzichè indicare dei range di versioni valide poichè
  questo non garantisce la replicabilità delle build degli applicativi/librerie (es. nel file `package.json` mai utilizzare i simboli `^` e `~`)
- È ammesso utilizzare i range per dichiarare le versioni delle librerie solamente nel caso serva ad indicare le versioni con le quali è assicurato
  il relativo funzionamento (es. quando si indicano le dipendenze di una libreria che garantisce compatibilità con diverse versioni di un framework)



