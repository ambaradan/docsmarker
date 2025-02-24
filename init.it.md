---
title: Initialization procedure
author: franco colussi
contributors: steve spencer
tags:
    - Neovim
    - editor
    - markdown
---
<!--vale off-->
## Introduzione

L'inizializzazione della configurazione di rocksmarker viene gestita nel file `init.lua` presente nella cartella principale. Il file `init.lua` è il file di configurazione di Neovim dove gli utenti possono definire le proprie impostazioni e personalizzazioni in Lua.

### Impostazioni personalizzate

La prima parte del file `init.lua` inserisce le opzioni e i comandi personalizzati usando la funzione ==require==, la funzione permette di integrare nella configurazione di Neovim del codice lua scritto su file esterni consentendo in questo modo la sua personalizzazione ed espansione.  
I comandi, o autocomandi, sono delle azioni che Neovim attua in base a determinati eventi, questi eventi sono codificati e sono consultabili sulla [documentazione di Neovim](https://Neovim.io/doc/user/autocmd.html#_5.-events). il loro utilizzo permette di creare dei comportamenti e delle funzioni di Neovim altrimenti non disponibili.

```lua linenums="2"
-- calls for input of options and autocommands
require("options")
require("commands")
```

### Procedura di bootstrap

Segue poi il codice fornito dal progetto rocks.nvim per la gestione del plugin stesso e dei successivi plugin, il codice è commentato in maniera esaustiva ma viene comunque fornito un riassunto delle operazioni compiute dai vari passaggi dello script alla fine del codice visualizzato sotto.

```lua title="rocks.nvim bootstrap" linenums="7" hl_lines="3 6 7 13 20 27 32 37 43 45"
do
    -- specifies where to install/use rocks.nvim
    local install_location = vim.fs.joinpath(vim.fn.stdpath("data") --[[@as string]], "rocks")

    -- set up configuration options related to rocks.nvim (recommended to leave as default)
    local rocks_config = {
        rocks_path = vim.fs.normalize(install_location),
    }

    vim.g.rocks_nvim = rocks_config

    -- configure the package path (so that plugin code can be found)
    local luarocks_path = {
        vim.fs.joinpath(rocks_config.rocks_path, "share", "lua", "5.1", "?.lua"),
        vim.fs.joinpath(rocks_config.rocks_path, "share", "lua", "5.1", "?", "init.lua"),
    }
    package.path = package.path .. ";" .. table.concat(luarocks_path, ";")

    -- configure the c path (so that e.g. tree-sitter parsers can be found)
    local luarocks_cpath = {
        vim.fs.joinpath(rocks_config.rocks_path, "lib", "lua", "5.1", "?.so"),
        vim.fs.joinpath(rocks_config.rocks_path, "lib64", "lua", "5.1", "?.so"),
    }
    package.cpath = package.cpath .. ";" .. table.concat(luarocks_cpath, ";")

    -- add rocks.nvim to the runtimepath
    vim.opt.runtimepath:append(vim.fs.joinpath(rocks_config.rocks_path, "lib", "luarocks", "rocks-5.1", "rocks.nvim", "*"))
end

-- if rocks.nvim is not installed then install it!
if not pcall(require, "rocks") then
    local rocks_location = vim.fs.joinpath(vim.fn.stdpath("cache") --[[@as string]], "rocks.nvim")

    if not vim.uv.fs_stat(rocks_location) then
        -- pull down rocks.nvim
        local url = "https://github.com/nvim-neorocks/rocks.nvim"
        vim.fn.system({ "git", "clone", "--filter=blob:none", url, rocks_location })
        -- make sure the clone was successfull
        assert(vim.v.shell_error == 0, "rocks.nvim installation failed. try exiting and re-entering Neovim!")
    end

    -- if the clone was successful then source the bootstrapping script
    vim.cmd.source(vim.fs.joinpath(rocks_location, "bootstrap.lua"))

    vim.fn.delete(rocks_location, "rf")
end
```

Lo script imposta la configurazione e l'ambiente necessari per utilizzare il plugin rocks.nvim in Neovim. In particolare esegue le seguenti operazioni:

- **Specifica il percorso di installazione di rocks.nvim**:  
Utilizzando la funzione ==vim.fs.joinpath()== imposta la variabile ==install_location== con il percorso di installazione di *rocks.nvim*. La cartella viene ricavata con l'uso della funzione `vim.fn.stdpath("data")` che fornisce il percorso standard usato da Neovim per la configurazione, per i dati e per i logs.
- **Imposta le opzioni di configurazione di rocks.nvim:**  
Crea la tabella ==rocks_config== con l'opzione ==rocks_path== impostata su *install_location* e imposta la variabile globale ==vim.g.rocks_nvim== sulla tabella *rocks_config*.
- **Configura il percorso del pacchetto lua:**  
Vengono aggiunti due percorsi alla variabile ==package.path==, che consente a Neovim di trovare i moduli lua installati da *rocks.nvim*.
- **Configura il percorso del pacchetto C:**  
Aggiunge due percorsi alla variabile ==package.cpath==, che consente a Neovim di trovare i moduli ==c== (ad esempio, i *parser tree-sitter*) installati da *rocks.nvim*.
- **Aggiunge rocks.nvim al percorso del runtime:**  
Aggiunge l'opzione ==vim.opt.runtimepath== al percorso del plugin *rocks.nvim*.
- **Installa rocks.nvim se non è già installato:**  
Lo script controlla se il modulo ==rocks== è disponibile. In caso contrario, procede all'installazione di rocks.nvim.  
Per prima cosa controlla se esiste la directory di installazione di rocks.nvim. In caso contrario, clona il repository rocks.nvim da GitHub usando la funzione ==vim.fn.system()==.  
Se il clone viene completato senza errori avvia lo script ==bootstrap.lua== dal repository clonato per completare l'installazione.  
Infine cancella la directory temporanea di installazione *rocks.nvim*.

Lo script in questo modo assicura la configurazione e l'ambiente necessario per il corretto funzionamento del plugin *rocks.nvim* in *Neovim*.

!!! Note "Sviluppare la propria configurazione"

    Lo script può essere usato anche per iniziare la propria personalizzazione dell'editor. Per farlo è sufficiente copiare lo script in un file chiamato `init.lua` nella cartella scelta per lo sviluppo posizionata in `~/.config/` e avviare Neovim usando la variabile ==NVIM_APPNAME==.  
    Il comando da utilizzare, supponendo che la cartella scelta sia `~/.config/rocks_test`, sarà:

    `NVIM_APPNAME=rocks_test nvim`

    Neovim creerà la configurazione partendo dalla cartella scelta anche per i dati in `~.local/share/rocks_test` e i dati temporanei `~/.cache/rocks_test`, impostando cioè una nuova configurazione totalmente indipendente che contiene solo il gestore dei plugin.  
    Da questo punto di partenza si può iniziare ad installare i plugin, configurare le opzioni e l'aspetto secondo preferenza.

## Conclusioni
