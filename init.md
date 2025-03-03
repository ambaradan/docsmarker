---
title: Init procedure
author: Franco colussi
contributors: Steve spencer
tags:
    - Neovim
    - editor
    - markdown
---

## Introduction

The initialization of the rocksmarker configuration is handled in the `init.lua` file located in the root directory. The `init.lua` file is the Neovim configuration file where users can define their own settings and customizations. It is written in *Lua* and allows for greater flexibility and power than traditional Vimscript configuration.

### Custom settings

The first part of the `init.lua` file inserts custom options and commands using the ==require== function; the function allows lua code written on external files to be integrated into Neovim's configuration, thus enabling its customization and expansion.  
Commands, or autocommands, are actions that Neovim implements based on certain events; these events are coded and can be found on the [Neovim documentation](https://Neovim.io/doc/user/autocmd.html#_5.-events). Their use allows the creation of behaviors and functions of Neovim that would otherwise be unavailable.

```lua linenums="2"
-- calls for input of options and autocommands
require("options")
require("commands")
```

### Bootstrap procedure

Then follows the code provided by the rocks.nvim project for managing the plugin itself and subsequent plugins, the code is commented on extensively but a summary of the operations performed by the various steps of the script is still provided at the end of the code displayed below.

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

The script sets up the configuration and environment required to use the rocks.nvim plugin in Neovim. Specifically, it performs the following operations:

- **Specifies the installation path of rocks.nvim**:  
Using the function *vim.fs.joinpath()* sets the variable *install_location* with the installation path of *rocks.nvim*. The folder is derived with the use of the *vim.fn.stdpath("data")* function, which provides the standard path used by Neovim to configure data.
- **Set rocks.nvim configuration options:**  
Create the *rocks_config* table with the *rocks_path* option set to *install_location* and set the global variable *vim.g.rocks_nvim* to the *rocks_config* table.
- **Configure lua package path:**  
Two paths are added to the *package.path* variable, which allows Neovim to find lua modules installed by *rocks.nvim*.
- **Configure the path to the C package:**  
Adds two paths to the *package.cpath* variable, which allows Neovim to find ==C== modules (e.g., *parser tree-sitters*) installed by *rocks.nvim*.
- **Adds rocks.nvim to the runtime path:**  
Adds the *vim.opt.runtimepath* option to the *rocks.nvim* plugin path.
- **Install rocks.nvim if not already installed:**  
The script checks if the *rocks* module is available. If not, it proceeds to install rocks.nvim.  
First check if the installation directory for rocks.nvim exists. If not, clone the rocks.nvim repository from GitHub using the *vim.fn.system()* function.  
If the clone completes without errors start the *bootstrap.lua* script from the cloned repository to complete the installation.  
Finally delete the temporary installation directory *rocks.nvim*.

The script in this way ensures the necessary configuration and environment for the proper functioning of the *rocks.nvim* plugin in *Neovim*.

!!! Note "Develop your own configuration"

    The script can also be used to start your own customization of the editor. To do this, simply copy the script provided by rocks.nvim to a file called `init.lua` in the folder chosen for development located in `~/.config/` and start Neovim using the variable ==NVIM_APPNAME==.  
    The command to use, assuming the chosen folder is `~/.config/rocks_test`, will be:

    `NVIM_APPNAME=rocks_test nvim`

    Neovim will create the configuration starting from the chosen folder also for the data in `~.local/share/rocks_test` and the temporary data `~/.cache/rocks_test`, i.e., setting up a new totally independent configuration containing only the plugin manager.  
    From this starting point you can begin to install plugins, configure options and appearance according to preference.

## Conclusions

The purpose of this script is to ensure that the *rocks.nvim* plugin is properly configured and available for use in the *Neovim* environment. The *rocks.nvim* plugin is a Neovim plugin that provides a convenient way to manage Lua dependencies and packages, similar to how [LuaRocks](https://luarocks.org) works for standalone *Lua* projects.

By running this script, the environment necessary for using *rocks.nvim* and associated packages is set up, and options and commands useful for managing the editor are also configured.
