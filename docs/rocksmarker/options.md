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

The `lua/options.lua` file allows for the definition and customization of the editor's configuration options in an efficient and organized manner. The options defined in this file directly affect the user experience.

Using the *options.lua* file allows for advanced customization of the editor without the need to modify the source code, keeping all configuration options in a single file to facilitate configuration management and maintenance.

## Options.lua in Rocksmarker

The **options.lua** file in the Rocksmarker project offers a set of default settings that enhance the Neovim user experience, covering aspects such as file management, text formatting, navigation, and user interface customization.

### Global configuration

This section defines global variables that affect the general behavior of Neovim.

: `vim.g.mapleader = vim.keycode("<Space>")`

: Set the `<Space>` key as the leader key. The leader key is a prefix used to define custom mappings (keyboard shortcuts). This means that all custom mappings defined in the configuration file will start with pressing the ++space++ key.

: `vim.g.markdown_recommended_style = 0`

: Disables the recommended style for Markdown. This avoids conflicts with the preferred style of the Rocksmarker project.

: `vim.g.loaded_python3_provider = 0`  
`vim.g.loaded_ruby_provider = 0`  
`vim.g.loaded_perl_provider = 0`  
`vim.g.loaded_node_provider = 0`

: Disable language providers for Python, Ruby, Perl, and Node.js. This reduces the overhead of these languages that are not necessary for the project, and avoids warnings produced by the `:checkhealth` command related to unmet dependencies for these providers.

### Editor options

This section configures the behavior of the text editor, affecting the display and interaction with files.

: `vim.o.foldenable = true`  

: Enable code folding, allowing you to collapse and expand sections of code. This can help reduce the amount of visible code and focus on the most relevant parts.

: `vim.o.foldmethod = "marker"`  

: Set the folding method to “marker”. This means that the determination of how folding occurs is by specific markers in the code. These markers are manually inserted by the user to define the sections of code to fold.

: `vim.o.foldmarker = "{{{,}}}"`

: Defines the markers used to delimit the folded sections of code. In this case, `{{{` marks the beginning and `}}}` marks the end of a foldable section.

: `vim.o.clipboard = "unnamedplus"`

: Configure integration with the system clipboard (Linux). The “unnamedplus” option allows for copying and pasting directly within the operating system clipboard, without the need to use specific Neovim commands.

: `vim.o.fillchars = "eob: "`

: Customize fill characters. In this case, it hides the ~ (tilde) character for empty lines at the end of the buffer, improving code readability.

: `vim.o.timeoutlen = 400`

: Set the wait time (in milliseconds) after each key press before considering it part of a command sequence. This affects the speed of command execution and keyboard sensitivity.

: `vim.o.undofile = true`

: Enables automatic saving of undo history, allowing you to undo changes even after closing and reopening the file. This is particularly useful for recovering earlier versions of the code.

: `vim.o.cursorline = true`

: Highlights the current line (the cursor location), helping you stay focused on your current position within the code.

: `vim.o.ignorecase = true`

: Set the search to ignore case sensitivity, simplifying the search for keywords within the code.

: `vim.o.smartcase = true`

: Change the behavior of `ignorecase` so that the search becomes *case-sensitive* only if using uppercase letters in the search pattern. This provides a balance between ease of searching and accuracy.

### Indentation settings

These options control how code is automatically indented.

: `vim.o.expandtab = true`

: Converts tab characters to spaces, helping to maintain consistent indentation and avoid compatibility issues between different text editors.

: `vim.o.tabstop = 4`

: Defines the number of spaces represented by a tab character (when *expandtab* is not enabled). A tab width of 4 spaces is a common convention for Markdown code.

: `vim.o.softtabstop = 4`

: Set the number of spaces inserted when you press the ++tab++ key. This value should match the tab width to maintain consistency in indentation.

: `vim.o.shiftwidth = 4`

: Defines the width of the indentation used by automatic indentation commands. Again, a width of 4 spaces is a common choice to support code readability.

: `vim.o.smartindent = true`

: Enable smart indentation, which automatically adjusts indentation based on the context of the code. This can help reduce the amount of manual work required to maintain correct indentation.

### User interface options

These options customize the appearance and behavior of Neovim's user interface.

: `vim.o.termguicolors = true`

: Enables the use of 24-bit colors (true color) in the terminal. Requires a terminal that supports true color to function properly.

: `vim.o.mouse = "a"`

: Enables mouse usage in all modes (normal, visual, insert, etc.). This can make Neovim more intuitive for users accustomed to graphical text editors.

: `vim.o.showmode = false`

: Hides the mode indicator (e.g., “INSERT,” “VISUAL”) from the status bar. This helps reduce the amount of information displayed and keeps your focus on the code.

: `vim.o.laststatus = 3`

: Set the status bar so that it is always visible globally. The status bar provides important information about the current state of Neovim, such as the active mode and cursor position.

: `vim.o.number = true`

: Show line numbers. Line numbers can be very useful for navigating through code and referring to specific lines during discussion or documentation.

: `vim.o.signcolumn = "yes"`

: Displays the marks column, used to display indicators such as debugger breakpoints or linting errors. You can customize the marks column to show different types of information.

: `vim.o.splitbelow = true`  
`vim.o.splitright = true`

: Configure the behavior of vertical and horizontal splits. Splits allow for the viewing of multiple files or parts of files simultaneously, improving productivity and code management.

### Session options

These options are specific to the *persisted.nvim* plugin, used for Neovim session saving and restoration.

: `vim.o.sessionoptions = "buffers,curdir,folds,globals,tabpages,winpos,winsize"`

: Defines what elements of the session to save or restore. In this case, open buffers, the current directory, fold status, global variables, tab pages, window position, and window size. With this you can restore the exact last working state when you restart Neovim.

## Conclusions

In summary, the *options.lua* file provides a complete and customized configuration for *Neovim*, covering aspects ranging from basic editor options to advanced settings for indentation, user interface, and session management. The design of this configuration is to improve productivity and the Neovim user experience, adapting it to the specific needs of authors and the *Rockmarker* project.

The use of options allows for the configuration of both the appearance and behavior of the editor to suit your specific needs. There are specific options for every aspect of Neovim, which you can find on the [relevant page](https://neovim.io/doc/user/options.html) of the Neovim documentation.
