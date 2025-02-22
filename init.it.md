---
title: Initialization procedure
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!--vale off-->
## Introduzione

```lua
do
    -- Specifies where to install/use rocks.nvim
    local install_location = vim.fs.joinpath(vim.fn.stdpath("data") --[[@as string]], "rocks")

    -- Set up configuration options related to rocks.nvim (recommended to leave as default)
    local rocks_config = {
        rocks_path = vim.fs.normalize(install_location),
    }

    vim.g.rocks_nvim = rocks_config

    -- Configure the package path (so that plugin code can be found)
    local luarocks_path = {
        vim.fs.joinpath(rocks_config.rocks_path, "share", "lua", "5.1", "?.lua"),
        vim.fs.joinpath(rocks_config.rocks_path, "share", "lua", "5.1", "?", "init.lua"),
    }
    package.path = package.path .. ";" .. table.concat(luarocks_path, ";")

    -- Configure the C path (so that e.g. tree-sitter parsers can be found)
    local luarocks_cpath = {
        vim.fs.joinpath(rocks_config.rocks_path, "lib", "lua", "5.1", "?.so"),
        vim.fs.joinpath(rocks_config.rocks_path, "lib64", "lua", "5.1", "?.so"),
    }
    package.cpath = package.cpath .. ";" .. table.concat(luarocks_cpath, ";")

    -- Add rocks.nvim to the runtimepath
    vim.opt.runtimepath:append(vim.fs.joinpath(rocks_config.rocks_path, "lib", "luarocks", "rocks-5.1", "rocks.nvim", "*"))
end

-- If rocks.nvim is not installed then install it!
if not pcall(require, "rocks") then
    local rocks_location = vim.fs.joinpath(vim.fn.stdpath("cache") --[[@as string]], "rocks.nvim")

    if not vim.uv.fs_stat(rocks_location) then
        -- Pull down rocks.nvim
        local url = "https://github.com/nvim-neorocks/rocks.nvim"
        vim.fn.system({ "git", "clone", "--filter=blob:none", url, rocks_location })
        -- Make sure the clone was successfull
        assert(vim.v.shell_error == 0, "rocks.nvim installation failed. Try exiting and re-entering Neovim!")
    end

    -- If the clone was successful then source the bootstrapping script
    vim.cmd.source(vim.fs.joinpath(rocks_location, "bootstrap.lua"))

    vim.fn.delete(rocks_location, "rf")
end
```

Lo script imposta la configurazione e l'ambiente necessari per utilizzare il plugin rocks.nvim in Neovim. In particolare esegue le seguenti operazioni:

- Specifica il percorso di installazione di rocks.nvim, la variabile ==install_location== viene impostata sul percorso in cui *rocks.nvim* verrà installato, utilizzando la funzione ==vim.fs.joinpath()== per costruire il percorso.
- Imposta le opzioni di configurazione di rocks.nvim, la tabella ==rocks_config== viene creata con l'opzione ==rocks_path== impostata su *install_location* mentre la variabile globale ==vim.g.rocks_nvim== è impostata sulla tabella *rocks_config*.
- Configura il percorso del pacchetto Lua, vengono aggiunti due percorsi alla variabile ==package.path==, che consente a Neovim di trovare i moduli Lua installati da *rocks.nvim*.
- Configura il percorso del pacchetto C, vengono aggiunti due percorsi alla variabile ==package.cpath==, che consente a Neovim di trovare i moduli ==C== (ad esempio, i parser tree-sitter) installati da *rocks.nvim*.
- Aggiunge ==rocks.nvim== al percorso del runtime, l'opzione ==vim.opt.runtimepath== viene aggiunta al percorso del plugin *rocks.nvim*.
- Installare rocks.nvim se non è già installato, lo script controlla se il modulo ==rocks== può essere caricato. In caso contrario, procede all'installazione di rocks.nvim.  
Per prima cosa controlla se la directory di installazione di rocks.nvim esiste. In caso contrario, clona il repository rocks.nvim da GitHub usando la funzione ==vim.fn.system()==.  
Se la clonazione ha esito positivo, il codice invia lo script bootstrap.lua dal repository clonato per completare l'installazione.
- Infine cancella la directory temporanea di installazione rocks.nvim.

Lo script in questo modo assicura che la configurazione e l'ambiente necessari siano impostati per l'uso del plugin *rocks.nvim* in *Neovim*. Gestisce l'installazione di *rocks.nvim* se non è già presente, rendendo conveniente per gli utenti iniziare a usare il plugin senza doverlo installare manualmente.
