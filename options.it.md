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
