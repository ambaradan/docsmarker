---
title: Options
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

## Introduction

A standard installation of Neovim comes ready to use but with a somewhat plain appearance and certainly not optimized for your specific preferences. The first step in customizing the installation is through configuring the options.  
Neovim's options are a powerful and flexible tool that allow you to modify many aspects of the editor, from the most basic such as displaying line numbers in the buffer to more advanced ones such as synchronizing the system clipboard or formatting settings.  
Their flexibility allows them to be applied at various levels, at the global level (==vim.g== - *global*) for general editor settings, at the window level (==vim.wo== - *window options*) for any work area settings, and at the buffer level (==vim.bo== - *buffer options*) for more granular customization.

To use the full potential of Neovim the configuration uses [ftplugin](https://neovim.io/doc/user/filetype.html#%3Afiletype-plugin-on) for configuring file types, consequently the specific options are found in the respective files in the `ftplugin` folder.  
This allows the settings to be tailored to each specific file type, streamlining the workflow and enabling a more organized and efficient coding environment.

## Global options

Rocksmarker's global settings are defined in the file `lua/options.lua` and are all commented out for a better understanding of their influence on Neovim's behavior. The main ones are those defined at the beginning of the file and are as follows:

```lua
-- Global configurations
vim.g.mapleader = vim.keycode("<Space>") -- leader key
vim.g.markdown_recommended_style = 0 -- disable Markdown recommended style
-- disable providers
-- remove warnings in ':checkhealth'
vim.g.loaded_python3_provider = 0
vim.g.loaded_ruby_provider = 0
vim.g.loaded_perl_provider = 0
vim.g.loaded_node_provider = 0
```

These settings determine the key to be used as the *leader key*. The *leader key* is the keyboard key that invokes all other key combinations, set to ++space+++.  

!!! Note ""

    In Rocksmarker, pressing it invokes the menu provided by *which-key.nvim*, which allows improved navigation between the commands provided and their descriptions.

This is followed by disabling the recommended style for markdown code *vim.g.markdown_recommended_style = 0* as it is not compatible with the Rocky Linux documentation.  
Embedded style mainly affects the formatting of code provided by the *language server*, a function that in Rocksmarker is handled by the *conform.nvim* plugin. For example, the built-in style sets the width of the document to 80 characters, and already this is not compatible with markdown written for documentation.

The next options have no effect on the editor configuration but are functional for debugging, they allow you to eliminate errors displayed by the ==:checkhealth== check referring to these languages that are not the subject of the project.

!!! Tip "Troubleshooting"

    The ==checkhealth== command along with the ==messages== command are indispensable tools for troubleshooting configuration problems.

### Editor options

The next options set the behavior of the editor in buffer management, behaviors such as search method, clipboard management and other aspects are set.

```lua
vim.o.clipboard = "unnamedplus" -- sync with the Linux clipboard
vim.o.mouse = "a" -- enable the use of the mouse - all modes
vim.o.timeoutlen = 400 -- how long wait after each keystroke before aborting it
vim.o.undofile = true -- automatically save undo history
vim.o.cursorline = true -- highlight the current line
vim.o.ignorecase = true -- to search case insensitively
vim.o.smartcase = true -- when the search pattern is typed
```

Indentation options are defined below; these are all set to four spaces as required for markdown code formatting.

```lua
-- Indenting
vim.o.expandtab = true -- Convert tabs to spaces
vim.o.tabstop = 4 -- Number of spaces a tab represents
vim.o.softtabstop = 4 -- how many spaces moves right when you press <Tab>
vim.o.shiftwidth = 4 -- provide proper indentation to the code
vim.o.smartindent = true -- increase/reduce the indent where appropriate
```

### Interface options

These options affect the appearance of the editor interface, integrating the buffer with features unique to a code editor. They also hide some default behaviors not needed by a markdown editor.

```lua
-- UI options
vim.o.termguicolors = true -- use true color
vim.o.fillchars = "eob:*" -- hide tilde '~' for blank lines
vim.o.showmode = false -- show the mode you are on the last line
vim.o.laststatus = 3 -- to global display the status line
vim.o.number = true -- enable line numbers
vim.o.signcolumn = "yes" -- displaying the signs
```

!!! Note ""

    The *vim.o.laststatus = 3* option is used to display a single *statusline* that would otherwise be displayed per buffer and then multiplied by the number of open buffers, a behavior more suitable for programming code and not necessary in a markdown editor.

The next options set the default behavior into which windows are divided, this setting is designed primarily for consulting Neovim's help pages, so the ==:help== command places the buffer to the right making it convenient to consult while editing code.

```lua
vim.o.splitbelow = true -- create a vertical split and open
vim.o.splitright = true -- new file in the right-hand side of the split
```

The options file ends with the definition of the *vim.o.sessionoptions* option; this option tells Neovim which parts of the editor should be saved in the session on exit. This information is used by the *persisted.nvim* plugin for session management.

```lua
-- require by persisted.nvim - enables saving and restoring
vim.o.sessionoptions = "buffers,curdir,folds,globals,tabpages,winpos,winsize"
```

## Conclusions

The use of options allows the editor to be configured in both appearance and behavior to suit specific needs; there are specific options for each aspect of Neovim that can be consulted on the [related page](https://neovim.io/doc/user/options.html) of the Neovim documentation.
