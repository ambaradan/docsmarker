---
title: Rocks.toml
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

## Introduction

**Rocks.toml** is a configuration file used by the *rocks.nvim* plugin to manage *Lua* dependencies.  
Allows you to specify the Lua packages (or "*rocks*") on which the Neovim configuration depends.

The main advantages of using a *rocks.toml* file with the rocks.nvim plugin are:

- **Dependency management**: The rocks.toml file allows you to centralize and manage the plugins used by the Neovim configuration. This makes it easier to keep them up-to-date and ensures that the configuration works consistently in different environments.
- **Reproducibility**: By specifying the exact versions of the plugins used, the rocks.toml file helps ensure that Neovim's configuration can be reproduced and shared with others without problems.
- **Automatic installation**: The rocks.nvim plugin automatically installs and manages the plugin dependencies specified in the rocks.toml file, so you don't have to worry about manually installing and configuring each dependency.

### Syntax

The *rocks.toml* file is a configuration file written in the TOML (Tom's Obvious, Minimal Language) format, which is a human-readable configuration file format. The file is usually placed in the root directory of the Lua project.

Shown below is an excerpt from the Rocksmarker configuration file as an introduction to writing it, this is only a small part of the file and its global consultation is recommended.  
To retrieve the configuration file in Rocksmarker use the command ==:Rocks edit==.

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

Explanation of syntax:

- The **config** table contains configuration options for the rocks.nvim plugin.
In this case, the **colorscheme** option is set to *bamboo*.
- The **plugins** table contains the list of Neovim plugins to be installed, with their versions. The *keys* in the plugins table are the plugin names and the *values* are the versions to be installed. If the plugin name contains a period (e.g., *rocks-config.nvim*), the key must be enclosed in quotation marks.  
The version can be a specific version number (e.g., "2.43.1") or a reference to the version control system (e.g., "scm" for the last commit of the source control system).  
There is then the possibility to disable a plugin by commenting it out (*# "rocks-git.nvim" = "2.5.2"*), running a subsequent ==:Rocks sync== this will be removed from the shared data folder and its management.

### Bundles

Bundles in Rocksmarker are used to define a set of plugins on which the configuration of Neovim is based. Bundles in *rocks.nvim* have the following functions:

- **Dependency management**:
Allows you to list the plugins that the Neovim configuration requires. By declaring dependencies in the related tables, you ensure that they are properly installed and available for use in the configuration.

- **Version constraints**:
Allows you to specify the desired versions or version ranges for each plugin to ensure compatibility. This helps avoid potential problems arising from incompatible versions or updates.

- **Custom configuration**:
It helps to define a custom configuration for each plugin, such as enabling or disabling functions, setting options, or adjusting behavior. This allows you to tailor the behavior of each plugin to your specific needs and preferences.

!!! tip ""

    Bundles are useful when you have a set of packages that are commonly used together in your project. By defining a bundle, you can install all the packages in the bundle in a single operation.

For example, this code can be used as a starting point for creating a Neovim development environment with the support of various programming languages and tools.

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

The code defines a table called **items** that contains a list of Neovim plugins commonly used to set up an LSP (*Language Server Protocol*) environment.

- The **mason.nvim** plugin is a package manager for Neovim's LSP servers, DAP servers, linters and formatters. It provides a unified interface for installing and managing these tools.
- The **mason-lspconfig.nvim** plugin integrates the *mason.nvim* package manager with the *nvim-lspconfig* plugin, allowing LSP servers to be easily installed and configured using the mason.nvim interface.
- The **nvim-lspconfig** plugin provides a set of configurations for various LSP servers, making it easier to install and configure different language servers in the system.
- The **nvim-cmp** plugin is a completion engine for Neovim, which provides an easy-to-use interface for viewing and selecting completion suggestions, allows you to view and select completion suggestions from various sources, including LSP servers.
- The **mason-tool-installer.nvim** plugin is an extension of the *mason.nvim* package manager, which allows you to automatically install and manage various development tools, such as linters, formatters and more.
- The **luasnip** plugin is a snippet engine for Neovim, providing a powerful and flexible way to create and manage code snippets, which can be used in conjunction with the *nvim-cmp* completion engine.

## Conclusions

The **rocks.toml** file enables simple and effective management of dependencies in Lua projects. By defining project dependencies in a centralized configuration file, you can ensure that your project has a consistent and reproducible set of dependencies, making it easier to develop, test, and deploy, and the *rocks.nvim* plugin allows you to take full advantage of these capabilities.
