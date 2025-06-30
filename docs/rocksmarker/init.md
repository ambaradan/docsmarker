---
title: Init procedure
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

## Introduction

The `init.lua` file is a configuration file that contains the settings and customizations for Neovim.  
When Neovim starts, this file runs automatically. Its function is to run a series of Lua commands and functions in sequence. These actions configure the Neovim editing environment.

## Init.lua in Rocksmarker

The *init.lua* file provided by Rocksmarker integrates a *bootstrap* configuration for the **rocks.nvim** plugin. The main goal of this file is to offer a basic configuration for *rocks.nvim* to easily install and manage Lua dependencies within Neovim. This includes management of libraries and plugins, and the configuration of the Neovim environment to allow smooth integration with installed dependencies.

### Key features

The *init.lua* file has a number of settings and configurations allowing you to:

* Specify the installation location of rocks.nvim
* Configure the options related to rocks.nvim
* Set Lua package path to allow Neovim to find and use installed libraries and plugins
* Configure C path to allow Neovim to find and use shared libraries
* Add rocks.nvim to the Neovim runtime path

See the [dedicated page](./rocks/bootstrap.md) for a detailed description of the *bootstrap* script.

### Calls to functions

In the final part of the *init.lua* file, there are three function calls used to call other Lua scripts to further configure the Neovim environment.  
These three function calls are essential to complete the configuration of Rocksmarker and enable additional features.

The three function calls at the bottom of the file are:

```lua
require("options")
require("commands")
require("mappings")
```

Each of these calls serves a specific purpose:

: `require("options")`

: The `require(“options”)` function calls the *options.lua* script, which contains the configuration settings. This script allows further customization of the Neovim environment, such as setting display, editing and search options.

: `require("commands")`

: The `require(“commands”)` function calls the *commands.lua* script, which defines custom autocmds. You can use these commands to perform specific actions within Neovim based on certain conditions such as file types, Neovim events, and more.

: `require("mappings")`

: The `require(“mappings”)` function calls the *mappings.lua* script, which defines key mappings. These mappings allow the assignment of specific actions to key combinations, simplifying interaction with the Neovim environment.

## Conclusions

The purpose of this script is to ensure that the *rocks.nvim* plugin is properly configured and available for use in the *Neovim* environment.  
By running this script, the set up of the environment necessary for the use of Rocksmarker, associated packages, and options and commands useful for managing the editor are also configured.
