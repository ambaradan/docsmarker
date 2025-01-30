---
title: Configuration
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

<!--vale off-->

# Configurazione di Rocksmarker

## Introduzione

La configurazione di Rocksmarker è gestita da **rocks.nvim**, un gestore di plugin di nuova concezione modulare e poco intrusivo.  
È stata utilizzata la funzionalità dei 'bundles', fornita da rocks.nvim, per raggruppare più plugin e le rispettive configurazioni, i file di configurazione inoltre sono stati creati in base alle funzionalità fornite.

## Struttura

Le configurazioni di tutti i plugin e delle opzioni per Neovim si trovano nella cartella `lua`, i due file *rocks.toml* e *init.lua* presenti nella cartella principale verranno trattati in seguito.

```txt
├── init.lua
├── lua
│   ├── commands.lua
│   ├── mappings.lua
│   ├── options.lua
│   └── plugins
│       ├── diagnostics.lua
│       ├── lsp.lua
│       ├── markdown.lua
│       ├── ui.lua
│       └── utils.lua
├── rocks.toml
```

### Configurazione globale

Sono presenti i file per le configurazioni globali di configurazione di Neovim:

`options.lua`

:    per le configurazioni generali come l'indentazione, la chiave principale per le mappature, gli spazi di tabulazione e altro

`commands.lua`

:    contiene gli autocomandi per le impostazioni automatiche di vari aspetti dell'editor

`mappings.lua`

:    per la mappatura delle scorciatoie da tastiera dei vari comandi forniti dall'editor e dai plugin

### Configurazioni plugin

I file di configurazione dei plugin si trovano nella nella cartella `lua/plugins`, questi file come detto in precedenza contengono le configurazioni cumulative di più plugin raggruppati per scopo. Per lo sviluppo di questa configurazione sono stati creati i seguenti file:

`ui.lua`

:    configurazioni necessarie alla corretta visualizzazione dell'interfaccia grafica

`utils.lua`

:    configurazioni dei plugin che forniscono le funzionalità proprie dell'editor come gestore dei file, delle sessioni, ricerca e sostituzione e altre utilità

`lsp.lua`

:    configurazioni per la corretta integrazione dei server linguistici (LSP), dei formattatori e dei linter

`diagnostic.lua`

:    configurazioni per la scrittura assistita come la segnalazione di errori, la corretta formattazione del codice e altro

`markdown.lua`

:    configurazioni dei plugin per la scrittura di codice *Markdown*, visualizzazione arricchita, modifica al volo degli attributi markdown e altro

!!! note "Separazione delle configurazioni"

    Rocks.nvim a differenza degli altri gestori di plugin separa la gestione delle installazioni da quella delle configurazioni e di consenguenza i file nella cartella `lua/plugins` non necessitano dei riferimenti iniziali sull'origine (repository GitHub) e le configurazioni non devono essere incluse in *tabelle lua* `{...}`

### Configurazione di rocks.nvim

I due file rimanenti della struttura sono i file utilizzati da rocks.nvim per la sua inizializzazione o installazione e per la gestione delle installazioni dei plugin. La configurazione dei plugin da installare è indipendente dalle configurazioni relative ed è interamente configurato in un solo file. Le funzioni dei due file sono:

`init.lua`

:    questo file è il file che inizializza tutta la configurazione, controlla la disponibilità delle versioni richieste di lua e luarocks e del plugin stesso e se non disponibile inizializza una nuova installazione attraverso una procedura di boostrap

`rocks.toml`

:     fornisce la configurazione del plugin stesso e di tutti i plugin aggiuntivi, questi sono inseriti automaticamente nel file se installati mediante il comando `:Rocks install <plugin_name>` ma possono anche essere configurati manualmente e poi installati con un `:Rocks sync`

Questa pagina è solo un'introduzione alla configurazione, per le descrizioni più approfondite fare riferimento alle pagine dedicate.
