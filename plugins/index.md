---
title: Configuration
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

# Configuration of Rocksmarker

## Introduction

Rocksmarker's configuration is managed by **rocks.nvim**, a newly developed modular and unobtrusive plugin manager.  
The functionality of 'bundles', provided by rocks.nvim, was used to group multiple plugins and their respective configurations; configuration files also were created according to the functionality provided.

## Structure

The configurations of all plugins and options for Neovim are found in the `lua` folder, the two files *rocks.toml* and *init.lua* found in the main folder will be covered later.

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

### Global configuration

The files for Neovim's global configurations are:

`options.lua`

: for general configurations such as indentation, main key for mappings, tab spaces, and other

`commands.lua`

: has autocommands for automatic settings of various aspects of the editor

`mappings.lua`

: for mapping keyboard shortcuts to the various commands provided by the editor and plugins

### Plugin configurations

The plugin configuration files are located in the `lua/plugins` folder, these files as mentioned earlier contain the cumulative configurations of multiple plugins grouped by purpose. The following files were created for the development of this configuration:

`ui.lua`

: configurations necessary for the correct display of the graphic interface

`utils.lua`

: plugin configurations that provide the editor's own features such as file manager, session manager, search and replace, and other utilities

`lsp.lua`

: configurations for the proper integration of language servers (LSPs), formatters, and linters

`diagnostic.lua`

: configurations for assisted writing such as error reporting, correct code formatting, and more

`markdown.lua`

: plugin configurations for writing *Markdown* code, enriched visualization, on-the-fly editing of markdown attributes, and more

!!! notes "Separation of configurations"

    Rocks.nvim unlike the other plugin managers separates the management of installations from the management of configurations and as a result the files in the `lua/plugins` folder do not need the initial references on the source (GitHub repository) and the configurations do not need to be included in *lua* `{...}` tables

### Configuration of rocks.nvim

The two remaining files in the structure are the files used by rocks.nvim for its initialization or installation and for managing plugin installations. The configuration of plugins to be installed is independent of related configurations and is entirely configured in one file. The functions of the two files are:

`init.lua`

: this file is the file that initializes all the configuration, checks for the availability of the required versions of lua and luarocks and the plugin itself and if not available initializes a new installation through a boostrap procedure

`rocks.toml`

: provides the configuration of the plugin itself and all additional plugins, these are automatically inserted into the file if installed via the `:Rocks install <plugin_name>` command but can also be manually configured and then installed with a `:Rocks sync`

This page is only an introduction to the configuration, for more in-depth descriptions refer to the dedicated pages.
