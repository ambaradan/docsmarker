---
title: Options
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!--vale off-->
## Impostazione delle opzioni

### Introduzione

Una installazione standard di Neovim si presenta già pronta all'uso ma con un aspetto un po' semplice e certamente non ottimizzata per le vostre preferenze specifiche. Il primo passo per la personalizzazione dell'installazione passa dalla configurazione delle opzioni.  
Le opzioni di Neovim sono uno strumento potente e flessibile che consentono di modificare numerosi aspetti dell'editor, dai più basilari come la visualizzazione dei numeri di riga nel buffer ad altre più avanzate come la sincronizzazione della clipboard del sistema o le impostazioni di formattazione.  
La loro flessibilità consente di applicarle a vari livelli, a livello globale (==vim.g== - *global*) per le impostazioni generali dell'editor, a livello di finestra (==vim.wo== - *window options*) per eventuali impostazioni di una area di lavoro e a livello di buffer (==vim.bo== - *buffer options*) per una personalizzazione più granulare.

Per sfruttare tutto il potenziale di Neovim la configurazione utilizza **ftplugin** per la configurazione dei tipi di file. Questo consente di adattare le impostazioni ad ogni tipo di file specifico snellendo il flusso di lavoro e permettendo così di lavorare in un ambiente di codifica più organizzato ed efficiente.

## Opzioni globali

Le impostazioni globali di Rocksmarker sono definite nel file `lua/options.lua` e sono tutte commentate per una maggiore comprensione della loro influenza sul comportamento di Neovim. Le principali sono quelle definite all'inizio del file e sono le seguenti:

```lua
-- Global configurations
vim.g.mapleader = vim.keycode("<Space>") -- leader key
vim.g.markdown_recommended_style = 0 -- disable Markdown recommended style
-- disable providers
-- remove warnings in ':checkhealth'
vim.g.loaded_python3_provider = 0
vim.g.loaded_ruby_provider = 0
vim.g.loaded_perl_provider = 0
vim.g.loaded_node_provider = 0
```

Queste impostazioni determinano la chiave da utilizzare come *leader key*. La *leader key* è la chiave della tastiera che richiama tutte le altre combinazioni di chiavi, impostata a ++space++.  

!!! Note ""

    In Rocksmarker la sua pressione richiama il menu fornito da *which-key.nvim* che permette una navigazione migliorata tra i comandi forniti e la descrizione degli stessi.

Segue poi la disabilitazione dello stile raccomandato per il codice markdown ==vim.g.markdown_recommended_style = 0== in quanto non compatibile con la documentazione Rocky Linux.  
Lo stile integrato influisce principalmente sulla formattazione del codice fornita dal *server linguistico*, funzione questa che in Rocksmarker è gestita dal plugin *conform.nvim*. Lo stile integrato ad esempio imposta la larghezza del documento a 80 caratteri e già questo non è compatibile con il markdown scritto per la documentazione.

Le successive opzioni non hanno alcun effetto sulla configurazione dell'editor ma sono funzionali al debug, permettono di eliminare gli errori visualizzati dal controllo ==:checkhealth== riferiti a questi linguaggi che non sono oggetto del progetto.

!!! Tip "Risoluzione dei problemi"

    Il comando ==checkhealth== insieme al comando ==messages== sono strumenti indispensabili per la risoluzione dei problemi di configurazione.

### Opzioni dell'editor

Le successive opzioni impostano il comportamento dell'editor nella gestione del buffer, sono impostati comportamenti come il metodo di ricerca, la gestione della clipboard e altri aspetti.

```lua
vim.o.clipboard = "unnamedplus" -- sync with the Linux clipboard
vim.o.mouse = "a" -- enable the use of the mouse - all modes
vim.o.timeoutlen = 400 -- how long wait after each keystroke before aborting it
vim.o.undofile = true -- automatically save undo history
vim.o.cursorline = true -- highlight the current line
vim.o.ignorecase = true -- to search case insensitively
vim.o.smartcase = true -- when the search pattern is typed
```

Di seguito sono definite le opzione di indentazione, queste sono impostate tutte a quattro spazi come richiesto per la formattazione di codice markdown.

```lua
-- Indenting
vim.o.expandtab = true -- Convert tabs to spaces
vim.o.tabstop = 4 -- Number of spaces a tab represents
vim.o.softtabstop = 4 -- how many spaces moves right when you press <Tab>
vim.o.shiftwidth = 4 -- provide proper indentation to the code
vim.o.smartindent = true -- increase/reduce the indent where appropriate
```

!!! Note ""

    L'opzione ==vim.o.smartindent== consente di formattare correttamente le liste markdown che altrimenti verrebbero indentate a quattro spazi.

### Opzioni dell'interfaccia

Queste opzioni influenzano l'aspetto dell'interfaccia dell'editor, integrano il buffer con le caratteristiche proprie di un editor di codice. Nascondono inoltre alcuni comportamenti di default non necessari ad un editor markdown.

```lua
-- UI options
vim.o.termguicolors = true -- use true color
vim.o.fillchars = "eob:*" -- hide tilde '~' for blank lines
vim.o.showmode = false -- show the mode you are on the last line
vim.o.laststatus = 3 -- to global display the status line
vim.o.number = true -- enable line numbers
vim.o.signcolumn = "yes" -- displaying the signs
```

L'opzione ==vim.o.laststatus = 3== serve a visualizzare una sola linea di stato che in caso contrario verrebbe visualizzata per buffer e quindi moltiplicata per il numero di buffer aperti.

Le successive opzioni impostano il comportamento predefinito in cui vengono divise le finestre, questa impostazione è pensata principalmente per la consultazione delle pagine di aiuto di Neovim, in questo modo il comando ==:help== posiziona il buffer a destra rendendo comoda la sua consultazione mentre si edita il codice.

```lua
vim.o.splitbelow = true -- create a vertical split and open
vim.o.splitright = true -- new file in the right-hand side of the split
```

Il file delle opzioni termina con la definizione dell'opzione ==vim.o.sessionoptions==, questa opzione indica a Neovim quali parti dell'editor debbano essere salvate nella sessione all'uscita. Queste informazioni vengono utilizzate dal plugin *persisted.nvim* per la gestione delle sessioni.

```lua
-- require by persisted.nvim - enables saving and restoring
vim.o.sessionoptions = "buffers,curdir,folds,globals,tabpages,winpos,winsize"
```

## Conclusioni
