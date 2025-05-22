---
title: Bootstrap
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!-- vale off -->

## Procedura di Bootstrap

### Introduzione

Il file *init.lua* fornisce una procedura di bootstrap per il plugin **rocks.nvim**. Questa procedura è responsabile dell'installazione e della configurazione di *rocks.nvim* all'interno dell'ambiente di *Neovim*. In questo capitolo, verrà descritta la procedura di bootstrap fornita dal file init.lua.

```lua title="rocks.nvim bootstrap" linenums="4"
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

#### Verifica dell'Installazione di rocks.nvim

La procedura di bootstrap inizia con la verifica dell'installazione di rocks.nvim. Ciò viene fatto utilizzando la funzione `pcall` per richiamare la funzione `require` con l'argomento "*rocks*". Se rocks.nvim non è installato, la funzione *pcall* restituirà un valore **nil** e la procedura di bootstrap proseguirà con l'installazione.

```lua
if not pcall(require, "rocks") then
    -- Installazione di rocks.nvim
end
```

#### Installazione di rocks.nvim

Se rocks.nvim non è installato, la procedura di bootstrap eseguirà l'installazione utilizzando la seguente sequenza di comandi:

* Verifica se la directory rocks.nvim esiste nella directory di cache di Neovim. Se non esiste, la procedura di bootstrap la creerà.
* Esegue il comando git clone per clonare il repository rocks.nvim dalla sua posizione ufficiale su GitHub.
* Verifica se la clonazione è stata eseguita con successo. Se non è stato possibile clonare il repository, la procedura di bootstrap si arresta e visualizza un messaggio di errore.

```lua
local rocks_location = vim.fs.joinpath(vim.fn.stdpath("cache"), "rocks.nvim")
if not vim.uv.fs_stat(rocks_location) then
    local url = "https://github.com/nvim-neorocks/rocks.nvim"
    vim.fn.system({ "git", "clone", "--filter=blob:none", url, rocks_location })
    assert(vim.v.shell_error == 0, "rocks.nvim installation failed. Try exiting and re-entering Neovim!")
end
```

#### Esecuzione dello Script di Bootstrap

Dopo l'installazione di rocks.nvim, la procedura di bootstrap eseguirà lo script di bootstrap **bootstrap.lua** presente nella directory rocks.nvim. Questo script è responsabile della configurazione finale di rocks.nvim e dell'abilitazione delle sue funzionalità.

```lua
vim.cmd.source(vim.fs.joinpath(rocks_location, "bootstrap.lua"))
```

#### Rimozione della Directory di Installazione

Infine, la procedura di bootstrap rimuove la directory di installazione rocks.nvim dalla directory di cache di Neovim.

```lua
vim.fn.delete(rocks_location, "rf")
```

## Conclusioni

In sintesi, la procedura di bootstrap è responsabile dell'installazione e della configurazione di rocks.nvim all'interno dell'ambiente di Neovim. Questa procedura verifica l'installazione di rocks.nvim, esegue l'installazione se necessario, esegue lo script di bootstrap e rimuove la directory di installazione.
