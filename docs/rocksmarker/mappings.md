---
title: Mappings
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

## Introduction

The file `lua/mapping.lua` is a key component in the configuration of Neovim. This file contains the definitions of the keyboard mappings, or the associations between key combinations and the performed actions that occur when you press those keys.

Keyboard mappings are essential for improving productivity and efficiency when working with Neovim. They allow performing complex operations with just a few keystrokes, reducing the time and effort required to complete tasks.  
In addition, you can customize keyboard mappings to fit your specific needs, making Neovim an even more comfortable and intuitive working environment.

Rocksmarker includes some dedicated functions in the file `lua/utils/editor.lua` to further simplify the writing of mappings. The purpose of the code is to define functions for handling key mappings within the editor. The **M.set_key_mapping** function in particular is the main interface for setting new key mappings.  
The defined functions also overcome the four-component limitation that normally restricts the use of **vim.keymap.set**, providing a more flexible and organized way to configure key mappings in Neovim.

## Mapping in Rocksmaker

You can find the code in the file `lua/utils/editor.lua`, used to collect all the custom functions dedicated to managing Rocksmarker. The functions are imported with the Lua function *require*:

```lua
local editor = require("utils.editor")
```

The included custom functions are:

```lua
--- Function to set key mappings with options
function M.set_key_mapping(mode, lhs, rhs, desc)
 local opts = M.make_opt(desc)
 M.remap(mode, lhs, rhs, opts)
end

--- Local function to remap keybinding.
M.remap = function(mode, lhs, rhs, opts)
 pcall(vim.keymap.del, mode, lhs)
 return vim.keymap.set(mode, lhs, rhs, opts)
end

--- Function to create default options for key mappings.
function M.make_opt(desc)
 return {
  silent = true,
  noremap = true,
  desc = desc,
 }
end
```

### Description

`M.set_key_mapping(mode, lhs, rhs, desc)`

: This function simplifies the creation of the key mapping. It receives the **mode** (mode - e.g., normal, insert, visual), **lhs** (the key combination you want to map), **rhs** (the command or action you want to perform) and **desc** (a description for the mapping). It finally converts the description (*desc)* to an option using *M.make_opt(desc)* and then invokes the subsequent function *M.remap(mode, lhs, rhs, opts)*.

`M.remap(mode, lhs, rhs, opts)`

: This function actually handles the key mapping. Initially, it uses *pcall* to attempt to remove any existing mappings with *vim.keymap.del*. This is important to avoid conflicts with previous mappings. Next, set the new mapping with **vim.keymap.set(mode, lhs, rhs, opts)**. Here, *opts* is the options object created in *M.make_opt(desc)*.

`M.make_opt(desc)`

: Creates and returns a set of basic options for key mappings. The specified options include:

- **silent**: to prevent action echo in the command line.
- **noremap**: to ensure that the mapping is not further interpreted, while maintaining its logic.
- **desc**: to provide a description, useful for getting help or documentation.

An example is as follows:

```lua
-- conform - manual formatting
editor.remap("n", "<leader>F", function()
 require("conform").format({ lsp_fallback = true })
end, editor.make_opt("format buffer"))
```

In this case, the use of the editor variable access two different functions:

- `remap` to create a new mapping defined for normal mode ("n"), and the key combination `<leader>F`. When you press this key combination, the editor performs formatting of the current buffer.

- `make_opt` to create a key mapping options object. In this case, the adding of "format buffer", such as *desc*, to the above options *silent* and *noremap* provides a brief explanation of the action performed by the mapping.

## Mappings set

### Buffer management

The buffer mappings defined in this section represent a set of customized commands for managing buffers in your work environment. These mappings improve file editing efficiency and productivity by allowing you to manage buffers in a more intuitive and personalized way.

: **Save current buffer**: `<C-s>`

: Saves the current buffer when pressing ++ctrl+"s"++ in insertion or normal mode.

: **Create a new buffer**: `<leader>b`

: Creates a new buffer when pressing ++space+"b"++ in normal mode.

: **Close current buffer**: `<leader>x`

: Closes the current buffer when pressing ++space+"x"++ in normal mode.

: **Close all buffers**: `<leader>X`

: Closes all buffers when pressing ++space+"X"++ in normal mode.

!!! tip "Use of `<leader>X`"

    Closing all open buffers in the editor with one command is very useful when working on multiple projects and you want to change projects without leaving the editor since the **persisted.nvim** plugin does not handle this. To avoid having files open in the previous session in the new session, you can remove them in one operation before switching sessions.

These mappings allow for the quick and efficient management of buffers in Neovim, without having to use the standard Neovim commands. For example, you can save the current buffer without having to type ++colon+"w "++ and press ++enter++.

### Editor commands

The mappings defined in the section represent a set of custom commands to enhance the editing experience in your work environment. You can use these mappings to perform operations such as formatting text, managing buffers, searching and replacing text, and accessing advanced features such as viewing diagnostics and managing symbols.

: **Quit editor**: `<leader>q`

: You can exit the current editor when you press ++space+"q"++ in normal mode.

: **Clear highlights**: `<Esc>`

: Clears search highlights when pressing ++esc++ in normal mode.

: **Telescope cmdline**: `,` (comma)

: Opens the command line in a Telescope buffer when pressing ++comma++ in normal mode.

: **Buffer formatting**: `<leader>F`

: Formats the current buffer when pressing ++space+"F"++ in normal mode.

: **Copy selected text**: `<C-c>` (in visual mode)

: Copies selected text to the system clipboard when pressing ++ctrl+"c"++ in visual mode.

: **Cuts selected text**: `<C-x>` (in visual mode)

: Cuts selected text and copies it to the system clipboard when pressing ++ctrl+"x"++ in visual mode.

: **Copy whole line**: `<S-c>` (in normal mode)

: Copies the whole row to the system clipboard when you press ++shift+"c"++ in normal mode.

: **Cut whole row**: `<S-x>` (in normal mode)

: Cuts the whole row and copies it to the system clipboard when you press ++shift+"x"++ in normal mode.

: **Paste text**: `<C-v>` (in normal and visual mode)

: Paste text from the system clipboard when pressing ++ctrl+"v"++ in normal or visual mode.

: **Paste text**: `<C-v>` (in insertion mode)

: Paste text from the system clipboard when pressing ++ctrl+"v"++ in insertion mode.

: **Paste without overwriting the registry**: `<CS-v>` (in visual mode)

: Pastes text without overwriting the register when pressing ++ctrl+shift+"v"++ in visual mode.

: **Move text block**: `J`and `K` (in visual mode)

: Move the selected text block up or down when pressing ++"J"++ or ++"K"++ in visual mode. Use the standard Neovim ++shift+"V"++ mapping to select the block you want to move.

: **Toggle diagnostics**: `<leader>dd`

: Activates or deactivates the virtual diagnostic text in the buffer when pressing ++space+"d"+"d"++ in normal mode.

### File management

These custom mappings help you make the most of Neo-tree's features, improving productivity and simplifying file and directory management within the editor. In this way you can work in a more efficient and organized manner, with complete visibility into your project's file and directory structure.

: **Open Neo-Tree in floating window**: `.` (comma)

: Opens and closes Neo-Tree in a floating window when you press ++"."++ in normal mode.

: **Open Neo-Tree in right window**: `<C-n>`

: Opens and closes Neo-Tree in a right window when pressing ++ctrl+"n"++ in normal mode.

: **Reveal current file in Neo-Tree**: `<leader>fr`

: Opens the current file in Neo-Tree when you press ++space+"f"+"r"++ in normal mode.

These mappings allow quick access to Neo-Tree's features and managing the file system in Neovim efficiently. For example, you can open Neo-Tree in a floating window without having to use the standard `:Neotree float` command.

### Bufferline

These mappings help manage buffers more quickly and intuitively, reducing the time spent navigating and managing buffers and allowing you to focus more on editing and development work. This improves productivity and workspace management, making the editor more efficient and easier to use.

: **Select buffer**: `<leader>bp`

: Allows selection of buffers when pressing ++space+"b"+"p"++ in normal mode. Upon pressing these keys, this replaces the buffer type logo with a character highlighted in red in the bufferline, and pressing the corresponding letter key brings the focus to the corresponding buffer.

: **Close selected buffer**: `<leader>bc`

: You can close an open buffer when you press ++space+"b"+"c"++ in normal mode. As with selection, this replaces the buffer logo and when you press the corresponding character, you close the buffer.

: **Cycles to next buffer**: `<TAB>`

: Switches to the next buffer when the pressing ++tab++ key in normal mode.

: **Cycles to previous buffer**: `<S-TAB>`

: Switches to the previous buffer when pressing ++shift+tab++ in normal mode.

These mappings allow for the quick and efficient management of buffers in Neovim, without having to use the standard Neovim commands. For example, you can move to the next or previous buffer without having to type `:bnext` or `:bprevious`.

### Search and preview

These custom mappings help make the most of Telescope's functionality by improving file management and navigation within the editor.

: **Buffer list**: `<leader>fb`

: Opens the Telescope window with the list of open buffers when you press ++space+"f"+"b"++ in normal mode.

: **File search**: `<leader>ff`

: Opens the Telescope window to search for files in the current directory when pressing ++space+"f"+"f"++ in normal mode.

: **Search for recent files**: `<leader>fo`

: Opens the Telescope window with the list of recently opened files when you press ++space+"f"+"o"++ in normal mode.

: **Frequent file search**: `<leader>fc`

: Opens the Telescope window with the list of most frequently used files when pressing ++space+"f"+"c"++ in normal mode.

: **Search within current buffer**: `<Leader>fz`

: Opens the Telescope window for searching within the current buffer when pressing ++space+"f"+"z"++ in normal mode.

You can use these mappings to perform file searches and operations in Neovim quickly and efficiently, without having to use standard Neovim commands. For example, you can open the Telescope window to search for the last modified files without having to type `:Telescope old_files`.

### Diagnostics

The mappings defined in this section represent a set of custom commands for handling diagnostics and problems in one's work environment. The design of these is to help users identify and resolve problems in their code more efficiently and quickly, thereby improving productivity and quality of work.

: **Toggle global diagnostics**: `<leader>dt`

: Turns global diagnostics on or off when pressing ++space+"d"+"t"++ in normal mode.

: **Toggle diagnostics of current buffer**: `<leader>db`

: Activates or deactivates current buffer diagnostics when pressing ++space+"d"+"b"++ in normal mode.

: **Toggle symbols of current buffer**: `<leader>ds`

: Activates or deactivates the display of current buffer symbols when pressing ++space+"d"+"s"++ in normal mode. This feature applied to Markdown code allows for navigation between file headers by reducing the time for positioning at the desired location.

These mappings allow for the handling of errors and diagnostics in Neovim quickly and efficiently, without having to use standard Neovim commands.

### Sessions

These customized mappings help manage work sessions more efficiently, allowing work to resume where it left off and maintain editor status between work sessions. This allows for more organized and productive work, with the ability to resume work at any time and maintain continuity of activities.

: **Select session**: `<A-s>`

: Opens the session selection window when pressing ++alt+"s"++ in normal mode.

: **Load last session**: `<A-l>`

: Loads the last saved session when pressing ++alt+"l"++ in normal mode.

: **Select session (alternative)**: `<leader>sS`

: Opens the session selection window when pressing ++space+"s"+"S"++ in normal mode.

: **Save current session**: `<leader>ss`

: Saves the current session when pressing ++space+"s"+"s"++ in normal mode.

: **Load last session (alternative)**: `<leader>sl`

: Loads the last saved session when pressing ++space+"s"+"l"++ in normal mode.

: **Stop current session**: `<leader>st`

: Stops the current session when pressing ++space+"s"+"t"++ in normal mode.

These mappings allow for the management of sessions in Neovim quickly and efficiently, without having to use the standard Neovim commands. For example, you can load the last saved session without having to type `:SessionLoadLast`.

### Search and Replacement

These customized mappings help simplify search and replacement operations, reducing the time spent searching for text. You can use these mapping to work more efficiently and accurately, with the ability to perform searches and substitutions quickly and individually, and to maintain control over the text and its formatting.

: **Enables/disables Spectre**: `<leader>R`

: Activates or deactivates the search and replace mode provided by *nvim-spectre* when pressing ++space+"R"++ in normal mode.

: **Current word search**: `<leader>rw`

: Performs a search for the current word in all files in the project when you press ++space+"r"+"w"++ in normal mode.

: **Search current word in file**: `<leader>rp`

: Performs a search for the current word in the current file when pressing ++space+"r"+"p"++ in normal mode.

You can use **Nvim-spectre** to search for text in the current file or in all files in the project, replace the found text with new text, and select the search mode, for example, searching for whole words or searching for regular expressions.  
It also offers other advanced features, such as the ability to search multiple files at once, perform directory and subdirectory searches, and exclude files or directories from the search.

### Differences

The design of these mappings is to keep track of changes made to files and to manage code versions more efficiently and quickly, improving collaboration and code management. With **diffview.nvim** it is possible to work more transparently and securely, always knowing the changes made to files and the code versions used.

: **Open Diffview**: `<leader>dv`

: Opens the Diffview window to display the differences between the current file and the previous version when pressing ++space+"d"+"v"++ in normal mode.

: **Opens repository history**: `<leader>dh`

: Opens the Diffview window to display the history of the current repository when pressing ++space+"d"+"h"++ in normal mode.

: **Open buffer history**: `<leader>df`

: Opens the Diffview window to display the current buffer history when pressing ++space+"d"+"f"++ in normal mode.

: **Close Diffview**: `<leader>dc`

: Closes the Diffview window when pressing ++space+"d"+"c"++ in normal mode. Diffview provides a dedicated command for closing it because the mapping for it is particularly complicated to accomplish with Neovim's mappings.

: **Compare with HEAD**: `<leader>dH`

: Opens the Diffview window to compare the current file with the HEAD version when pressing ++space+"d"+"H"++ in normal mode.

You can use these mappings to view the differences between files and buffers in Neovim quickly and efficiently, without having to use the standard Neovim commands. For example, you can open the Diffview window to view the differences between the current file and the previous version without having to type `:DiffviewOpen`.

### Repository Git

These mappings enable operations such as opening git management for the workspace or current buffer, viewing commit history, and accessing git's advanced features. You can use these mappings to help you manage code versioning more efficiently and quickly.

: **Open Neogit**: `<leader>gm`

: Opens Neogit, a user interface for Git, when you press ++space+"g"+"m"++ in normal mode.

: **Open Neogit for current buffer**: `<leader>gM`

: Opens Neogit for the current buffer when pressing ++space+"g"+"M"++ in normal mode.  

: You can use these to apply changes to files outside the current workspace without having to leave the session.

: **View commit history**: `<leader>gh`

: Displays the repository's commit history when pressing ++space+"g"+"h"++ in normal mode.

: **View the commit history of the current buffer**: `<leader>gb`

: Displays the commit history of the current buffer when pressing ++space+"g"+"b"++ in normal mode.

Git mapping allow for the management of Git repositories directly within Neovim, view the history of repository commits and buffers, and run Git commands directly from Neovim.

### Terminal

The terminal mappings defined in this section represent a set of custom commands for managing the terminal window in your work environment. With these mappings, users can work in a more integrated way with the terminal, performing system and code management tasks more easily and quickly.

: **Toggle Horizontal terminal**: `<a-t>`

: Allows for the opening and closing of the terminal in horizontal mode. When pressing ++alt+"t"++ in normal, insertion, or terminal mode, the terminal opens or closes in horizontal mode.

: **Toggle vertical terminal**: `<a-v>`

: Allows for the opening and closing of the terminal in vertical mode. When you press ++alt+"v"++ in normal, insertion, or terminal mode, the terminal opens or closes in vertical mode.

: **Toggle float terminal**: `<a-f>`

: Allows for the opening and closing of the terminal in float mode. When you press ++alt+"f"++ in normal, insertion, or terminal mode, the terminal opens or closes in float mode.

### Logs Management

The mapping defined in this section manages notifications and status messages in your working environment. You can use this mapping to view notifications and status messages from running processes, and to manage the status information of plugins and extensions.

: **View messages**: `<leader>lg`

: Allows for the viewing of system messages passed to **fidget.nvim**. When you press ++space+"l"+"g"++ in normal mode, a window with editor and plugin status messages opens.

## Conclusions

In conclusion, the mappings presented in this page represent a comprehensive and customized collection of commands and shortcuts to enhance the experience of using the code editor. Here the `editor.remap()` function is extensively used to build these mappings, offering great flexibility and customization in defining custom commands.

You can use the `editor.remap()` function to redefine existing commands or create new ones, assigning them specific actions and custom descriptions. You can use these to create a collection of mappings that cover a wide range of functionality, from basic operations such as saving and closing buffers, to more advanced features such as file management, text search and replace, and Git commit management.
