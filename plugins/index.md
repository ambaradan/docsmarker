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

**rocks.nvim**, a newly developed, modular, and unobtrusive plugin manager, manages Rocksmarker's configuration.  
The functionality of 'bundles', that rocks.nvim provides, groups multiple plugins and their associated configurations. There are configuration files provided according to their functionality.

## Structure

You can find configurations for all plugins and options for Neovim in the `lua` folder. The two files, *rocks.toml* and *init.lua* found in the main folder, are exceptions discussed later.

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

The plugin configuration files are in the `lua/plugins` folder. These files, as mentioned earlier, contain the cumulative configurations of multiple plugins grouped by purpose. The following files are for the development of this configuration:

`ui.lua`

: configurations necessary for the correct display of the graphic interface

`utils.lua`

: plugin configurations that provides the editor's own features, such as file manager, session manager, search and replace, and other utilities

`lsp.lua`

: configurations for the proper integration of language servers (LSPs), formatters, and linters

`diagnostic.lua`

: configurations for assisted writing such as error reporting, correct code formatting, and more

`markdown.lua`

: plugin configurations for writing *Markdown* code, enriched visualization, on-the-fly editing of markdown attributes, and more

!!! note "Separation of configurations"

    Rocks.nvim, unlike the other plugin managers, separates the management of installations from the management of configurations, and as a result the files in the `lua/plugins` folder do not need the initial references on the source (GitHub repository) and you do not need to include the configurations in *lua* `{...}` tables.

### Configuration of rocks.nvim

The two remaining files in the structure are for use by rocks.nvim for its initialization or installation and for managing plugin installations. The configuration of plugins for installation is independent of related configurations and is entirely done in one file. The functions of the two files are:

`init.lua`

: this file is the file that initializes all the configuration, checks for the availability of the required versions of lua and luarocks and the plugin itself and if not available initializes a new installation through a boostrap procedure

`rocks.toml`

: provides the configuration of the plugin itself and all additional plugins, these are automatically inserted into the file if installed via the `:Rocks install <plugin_name>` command but can also be manually configured and then installed with a `:Rocks sync`

This page is only an introduction to the configuration, for more in-depth descriptions see the dedicated pages.
