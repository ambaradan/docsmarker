---
title: Introduzione
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!--vale off-->
## Neovim IDE for Markdown

Il progetto mira a costruire una configurazione di Neovim ottimizzata per la scrittura di documentazione in formato Markdown con un'installazione standard di Neovim come base. Questo progetto utilizza un progetto giovane ma molto promettente, rocks.nvim, per la gestione dei plugin. L'idea è nata dalla curiosità di sviluppare una configurazione di Neovim che non fosse gestita da lazy.nvim, il gestore “de facto” di tutte le principali configurazioni attualmente disponibili (NvChad, LunarVim, LazyVim e altri).

### Motivo del cambio di gestore

**Lazy.nvim** è un buon strumento per gestire i plugin di Neovim, ma presenta alcune limitazioni. Per funzionare correttamente, richiede la completa disabilitazione dei plugin integrati di Neovim, rendendo ad esempio molto complicata l'integrazione di funzioni di localizzazione (come l'ortografia). Al contrario, *rocks.nvim* è molto meno invasivo e non richiede alcuna impostazione speciale della configurazione di base di Neovim.

**Rocks.nvim** utilizza un file centralizzato per le operazioni sui plugin (installazione, rimozione, aggiornamento), separando queste operazioni dalla logica di configurazione dei plugin stessi e rendendone più semplice la gestione. Anche *lazy.nvim* include un meccanismo per escludere il plugin dalla configurazione, ma deve essere impostato manualmente nel file di configurazione corrispondente, rendendo la gestione più complicata.

## Componenti principali

* **Neovim** - moderno editor di testo per terminali nato da un fork di Vim che, pur condividendone le caratteristiche di base, ne amplia l'estensibilità, consentendo di personalizzare ulteriormente Neovim in base alle proprie esigenze (plugin, aspetto e così via).
* **rocks.nvim** - gestore di plugin per Neovim che mira a rivoluzionare la gestione dei plugin di Neovim semplificando il modo in cui gli utenti e gli sviluppatori gestiscono i plugin e le dipendenze, integrandosi direttamente con luarocks

**Neovim** è un progetto che non ha bisogno di presentazioni. Fornisce un editor stabile e personalizzabile ad alte prestazioni, utilizzabile per lo sviluppo di praticamente qualsiasi linguaggio di programmazione. Il linguaggio utilizzato per costruirlo (lua) lo rende portabile su più architetture ed estremamente veloce. L'uso di lua consente di iniettare blocchi di codice per modificarne le proprietà, come l'aspetto, la funzionalità e così via.

**Rocks.nvim** è un progetto giovane ma in crescita e il suo utilizzo si è dimostrato stabile. Le caratteristiche principali includono:

* Interfaccia utente minimale e non intrusiva
* Gestione automatica delle dipendenze e degli script di compilazione
* Completamento dei comandi
* Plugin forniti in versione binaria, quindi senza necessità di compilazione, tratti da [luarocks.org](https://luarocks.org/)
* Modularità

!!! note "Modularità dei plugin"

    Quest'ultimo aspetto è molto interessante. È possibile, partendo dal plugin principale rocks.nvim, estenderne le funzionalità attraverso plugin aggiuntivi che introducono nuove possibilità di configurazione e installazione. La modularità permette anche di escludere dalla configurazione il codice inutilizzato che potrebbe creare problemi o conflitti.

## Aspetto e interfaccia utente

L'editor fornisce un'unica interfaccia basata sul tema [bamboo.nvim](https://github.com/ribru17/bamboo.nvim) con tonalità verde scuro, scritto in Lua con l'evidenziazione della sintassi di Tree-sitter e l'evidenziazione semantica di LSP. Lo stile è stato applicato anche ai plugin aggiuntivi per conformarsi all'interfaccia.  
La creazione di un insieme di colori in `lua/plugins/bamboo.lua` facilita il riconoscimento dei marcatori senza essere invasivo.

## Componenti addizionali

L'integrazione comprende vari plugin per la gestione del flusso di lavoro. Il flusso di lavoro consiste principalmente nella scrittura o nella modifica di documenti Markdown, pertanto sono disponibili strumenti per la gestione dei file, la ricerca e la sostituzione di parole o frasi, la gestione dei file in un repository *Git* e altre utilità.

## Markdown set

Nella configurazione di questo progetto sono inclusi tutti i plugin disponibili in Neovim per la manipolazione e la visualizzazione di documenti markdown. I plugin sono stati accuratamente testati per funzionare con *rocks.nvim* e, se non ancora disponibili nella versione *luarocks*, l'installazione è avvenuta con l'utility aggiuntiva *rocks-git.nvim*.  
È stato inoltre creato un autocomando, attivato dal tipo di file, che imposta le caratteristiche del buffer per ottimizzare la scrittura del codice markdown.

## Riconoscimenti

Un grande ringraziamento va agli sviluppatori di NvChad per l'eccellente codice prodotto che è servito come studio e ispirazione per la stesura di questa configurazione. Un ringraziamento va anche agli sviluppatori di *rocks.nvim*, che hanno portato una ventata di aria fresca nella gestione dei plugin di Neovim, e a tutti gli sviluppatori dei plugin utilizzati.
