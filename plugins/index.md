---
title: Layout
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
The ==bundles== functionality, provided by *rocks.nvim*, was used to group multiple plugins and their respective configurations, the configuration files also were created according to the functionality provided.

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

Files for Neovim's global configuration settings are present:

`options.lua`

: For general configurations such as indentation, master key for mappings, tab spaces and more.

`commands.lua`

: Contains autocommands for automatic settings of various aspects of the editor such as custom settings based on file type, creating missing paths when creating a new file, and other useful features.

`mappings.lua`

: Used for mapping keyboard shortcuts of the various commands provided by the editor and plugins. The file contains the mappings of all plugins set in the configuration and can be used as a starting point for the memorization of the mappings.

### Plugin configurations

The plugin configuration files are located in the `lua/plugins` folder, these files as mentioned earlier contain the cumulative configurations of multiple plugins grouped by purpose. The following files were created for the development of this configuration:

`ui.lua`

: Configurations necessary for the correct display of the GUI.

`utils.lua`

: Plugin configurations that provide the editor's own features such as file manager, session manager, search and replace, and other utilities.

`lsp.lua`

: Configurations for proper integration of language servers (LSPs), formatters, and linters.

`diagnostic.lua`

: Configurations for assisted writing such as error reporting, correct code formatting, and more.

`markdown.lua`

: Plugin configurations for writing *Markdown* code, enriched visualization, on-the-fly editing of markdown attributes, and more.

!!! note "Separation of configurations"

    Rocks.nvim unlike the other plugin managers separates the management of installations from the management of configurations and as a result the files in the `lua/plugins` folder do not need the initial references on the source (GitHub repository) and the configurations do not need to be included in *lua* `{...}` tables

### Configuration of rocks.nvim

The two remaining files in the structure are the files used by *rocks.nvim* for its initialization or installation and for managing plugin installations. The configuration of plugins to be installed is independent of related configurations and is entirely configured in one file `rocks.toml`. The functions of the two files are:

`init.lua`

: This file is the file that initializes all configuration, checks for the availability of the required versions of *lua* and *luarocks* and the plugin itself, and if not available initializes a new installation through a bootstrap procedure.

`rocks.toml`

: Provides configuration of the plugin itself and all additional plugins, these are automatically placed in the file if installed using the `:Rocks install <plugin_name>` command but can also be manually configured and then installed with a `:Rocks sync`.

## Conclusions

This introductory page provides an overview of the configuration organization. To go into detail about the various components, the relevant pages can be consulted.
