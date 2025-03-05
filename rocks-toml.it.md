---
title: rocks.toml
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!--vale off-->
## Introduzione

**Rocks.toml** è un file di configurazione usato dal plugin *rocks.nvim* per gestire le dipendenze *Lua*.  
Consente di specificare i pacchetti Lua (o “*rocks*”) da cui dipende la configurazione di Neovim.

I vantaggi principali dell'utilizzo di un file *rocks.toml* con il plugin rocks.nvim sono:

- **Gestione delle dipendenze**: Il file rocks.toml consente di centralizzare e gestire le dipendenze Lua utilizzate dalla configurazione di Neovim. Ciò rende più facile mantenere le dipendenze aggiornate e garantisce che la configurazione funzioni in modo coerente in ambienti diversi.
- **Riproducibilità**: Specificando le versioni esatte dei pacchetti Lua utilizzati, il file rocks.toml aiuta a garantire che la configurazione di Neovim possa essere riprodotta e condivisa con altri senza problemi.
- **Installazione automatica**: Il plugin rocks.nvim installa e gestisce automaticamente le dipendenze Lua specificate nel file rocks.toml, in modo da non doversi preoccupare di installare e configurare manualmente ogni dipendenza.

### Sintassi

Il file *rocks.toml* è un file di configurazione scritto nel formato TOML (Tom's Obvious, Minimal Language), che è un formato di file di configurazione leggibile dall'uomo. Il file è solitamente collocato nella directory principale del progetto Lua.

Qui sotto è mostrato un estratto del file di configurazione di Rocksmarker come introduzione alla sua scrittura, questa è solo una piccola parte del file e si consiglia la sua consultazione globale.  
Per richiamare il file di configurazione in Rocksmarker utilizzare il comando ==:Rocks edit==.

```toml
[config]
colorscheme = "bamboo"
# List of Neovim plugins to install alongside their versions.
# If the plugin name contains a dot then you must add quotes to the key name!
[plugins]
"rocks.nvim" = "2.43.1"
"rocks-config.nvim" = "3.1.0"
# "rocks-git.nvim" = "2.5.2"
"rocks-treesitter.nvim" = "1.3.0"
"rocks-edit.nvim" = "scm"
```

Spiegazione della sintassi:

- La tabella **config** contiene le opzioni di configurazione del plugin rocks.nvim.
In questo caso, l'opzione **colorscheme** è impostata su *bamboo*.
- La tabella **plugins** contiene l'elenco dei plugin Neovim da installare, con le relative versioni. Le *chiavi* della tabella dei plugin sono i nomi dei plugin e i *valori* sono le versioni da installare. Se il nome del plugin contiene un punto (ad esempio, *rocks-config.nvim*), la chiave deve essere racchiusa tra virgolette.  
La versione può essere un numero di versione specifico (ad esempio, “2.43.1”) o un riferimento al sistema di controllo delle versioni (ad esempio, “scm” per l'ultimo commit del sistema di controllo dei sorgenti).  
Esiste poi la possibilità di disattivare un plugin commentandolo (*# "rocks-git.nvim" = "2.5.2"*), eseguendo un successivo ==:Rocks sync== questo verrà eliminato dalla cartella condivisa dei dati e dalla sua gestione.

### Bundles

I bundle in Rocksmarker sono usati per definire un insieme di pacchetti esterni (*rocks*) su cui si basa la configurazione di Neovim. I bundle in rocks.nvim hanno le seguenti funzioni:

- **Gestione delle dipendenze**:  
Consente di elencare i pacchetti esterni che la configurazione di Neovim richiede. Dichiarando le dipendenze nelle tabelle relative, ci si assicura che siano correttamente installate e disponibili per l'uso nella configurazione.

- **Vincoli di versione**:  
Permette di specificare le versioni o gli intervalli di versioni desiderati per ogni dipendenza, per garantire la compatibilità con la configurazione di Neovim. Questo aiuta a evitare potenziali problemi derivanti da versioni o aggiornamenti incompatibili.

- **Configurazione personalizzata**:  
Aiuta a definire una configurazione personalizzata per ogni dipendenza, come l'abilitazione o la disabilitazione di funzioni, l'impostazione di opzioni o la regolazione del comportamento. In questo modo è possibile adattare il comportamento di ogni pacchetto alle proprie esigenze e preferenze specifiche.

!!! tip ""

    I bundle sono utili quando si ha un insieme di pacchetti che vengono comunemente usati insieme nel progetto. Definendo un bundle, è possibile installare tutti i pacchetti del bundle con una singola operazione.

Ad esempio questo codice può essere utilizzato come punto di partenza per la creazione di un ambiente di sviluppo Neovim con il supporto di vari linguaggi e strumenti di programmazione.

```lua
[bundles.lsp]
items = [
    "mason.nvim",
    "mason-lspconfig.nvim",
    "nvim-lspconfig",
    "nvim-cmp",
    "mason-tool-installer.nvim",
    "luasnip",
]
```

Il codice definisce una tabella chiamata **items** che contiene un elenco di plugin Neovim comunemente usati per impostare un ambiente LSP (*Language Server Protocol*).

- Il plugin **mason.nvim** è un gestore di pacchetti per i server LSP, i server DAP, i linters e i formattatori di Neovim. Fornisce un'interfaccia unificata per l'installazione e la gestione di questi strumenti.
- Il plugin **mason-lspconfig.nvim** integra il gestore di pacchetti *mason.nvim* con il plugin *nvim-lspconfig*, consentendo di installare e configurare facilmente i server LSP utilizzando l'interfaccia mason.nvim.
- Il plugin **nvim-lspconfig** fornisce un insieme di configurazioni per vari server LSP, rendendo più facile l'installazione e la configurazione di diversi server linguistici nel  sistema.
- Il plugin **nvim-cmp** è un motore di completamento per Neovim, che fornisce un'interfaccia di facile utilizzo per la visualizzazione e la selezione di suggerimenti di completamento, consente di visualizzare e selezionare suggerimenti di completamento da varie fonti, compresi i server LSP.
- Il plugin **mason-tool-installer.nvim** è un'estensione del gestore di pacchetti *mason.nvim*, che consente di installare e gestire automaticamente vari strumenti di sviluppo, come linters, formattatori e altro.
- Il plugin **luasnip** è un motore di snippet per Neovim, che fornisce un modo potente e flessibile per creare e gestire snippet di codice, che possono essere utilizzati insieme al motore di completamento *nvim-cmp*.

## Conclusioni

Il file **rocks.toml** consente di gestire in modo semplice ed efficace le dipendenze nei progetti Lua. Definendo le dipendenze del progetto in un file di configurazione centralizzato, si può garantire che il progetto abbia un insieme coerente e riproducibile di dipendenze, rendendo più facile lo sviluppo, il test e la distribuzione e il plugin *rocks.nvim* consente di sfruttare pienamente queste funzionalità.
