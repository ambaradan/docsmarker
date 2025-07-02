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

The file `lua/mapping.lua` is a key component in the configuration of Neovim. This file contains the definitions of the keyboard mappings, that is, the associations between key combinations and the actions that are to be performed when they are pressed.

Keyboard mappings are essential for improving productivity and efficiency when working with Neovim. They allow complex operations to be performed with just a few keystrokes, reducing the time and effort required to complete tasks.  
In addition, keyboard mappings can be customized to fit the specific needs of the user, making Neovim an even more comfortable and intuitive working environment.

To further simplify the writing of mappings in Rocksmarker, some dedicated functions are provided in the file `lua/utils/editor.lua`. The purpose of the code is to define functions for handling key mappings within the editor. The **M.set_key_mapping** function in particular is the main interface for setting new key mappings.  
The defined functions also overcome the four-component limitation that normally restricts the use of **vim.keymap.set**, providing a more flexible and organized way to configure key mappings in Neovim.

## Mapping in Rocksmaker

The code can be found in the file `lua/utils/editor.lua`, this file is used to collect all the custom functions dedicated to managing Rocksmarker. The functions are imported with the Lua function *require*:

```lua
local editor = require("utils.editor")
```

And they are as follows:

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

: This function simplifies the creation of the key mapping, it receives the **mode** (mode - e.g., normal, insert, visual), **lhs** (the key combination you wish to map), **rhs** (the command or action to be performed) and **desc** (a description for the mapping). It finally converts the description (*desc)* to an option using *M.make_opt(desc)* and then invokes the subsequent function *M.remap(mode, lhs, rhs, opts)*.

`M.remap(mode, lhs, rhs, opts)`

: This function actually handles the key mapping. Initially, it uses *pcall* to attempt to remove any existing mappings using *vim.keymap.del*. This is important to avoid conflicts with previous mappings. Next, set the new mapping using **vim.keymap.set(mode, lhs, rhs, opts)**. Here, *opts* is the options object created in *M.make_opt(desc)*.

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

In this case the editor variable is used to access two different functions:

- `remap` to create a new mapping defined for normal mode ("n"), and the key combination `<leader>F`. When this combination is pressed, formatting of the current buffer is performed.

- `make_opt` to create a key mapping options object. In this case, "format buffer" such as *desc* is added to the above options *silent* and *noremap* to provide a brief explanation of the action performed by the mapping.

## Mappings set

### Buffer management

The buffer mappings defined in this section represent a set of customized commands for managing buffers in your work environment. These mappings improve file editing efficiency and productivity by allowing you to manage buffers in a more intuitive and personalized way.

: **Save current buffer**: `<C-s>`

: Saves the current buffer when pressing ++ctrl+"s"++ in insertion or normal mode.

: **Create a new buffer**: `<leader>b`

: Creates a new buffer when ++space+"b"++ is pressed in normal mode.

: **Close current buffer**: `<leader>x`

: Closes the current buffer when ++space+"x"++ is pressed in normal mode.

: **Close all buffers**: `<leader>X`

: Closes all buffers when pressing ++space+"X"++ in normal mode.

!!! tip "Use of `<leader>X`"

    Closing all open buffers in the editor with one command is very useful when working on multiple projects and you want to change projects without leaving the editor since the **persisted.nvim** plugin does not handle this. So to avoid having in the new session also the files open in the previous one you can remove them in one operation before switching sessions.

These mappings allow you to manage buffers in Neovim quickly and efficiently, without having to use the standard Neovim commands. For example, you can save the current buffer without having to type ++colon+"w "++ and press ++enter++.

### Editor Commands

The mappings defined in the section represent a set of custom commands to enhance the editing experience in your work environment. These mappings allow you to perform operations such as formatting text, managing buffers, searching and replacing text, and accessing advanced features such as viewing diagnostics and managing symbols.

: **Quit editor**: `<leader>q`

: Allows you to exit the current editor when you press ++space+"q"++ in normal mode.

: **Clear highlights**: `<Esc>`

: Clears search highlights when pressing ++esc++ in normal mode.

: **Telescope cmdline**: `,` (comma)

: Opens the command line in a Telescope buffer when pressing ++comma++ in normal mode.

: **Buffer formatting**: `<leader>F`

: Formats the current buffer when pressing ++space+"F"++ in normal mode.

: **Copy selected text**: `<C-c>` (in visual mode)

: Copies selected text to the system clipboard when ++ctrl+"c"++ is pressed in visual mode.

: **Cuts selected text**: `<C-x>` (in visual mode)

: Cuts selected text and copies it to the system clipboard when ++ctrl+"x"++ is pressed in visual mode.

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

: Move the selected text block up or down when pressing ++"J"++ or ++"K"++ in visual mode. Use the standard Neovim ++shift+"V"++ mapping to select the block to be moved.

: **Toggle diagnostics**: `<leader>dd`

: Activates or deactivates the virtual diagnostic text in the buffer when ++space+"d"+"d"++ is pressed in normal mode.

### File management

These custom mappings help you make the most of Neo-tree's features, improving productivity and simplifying file and directory management within the editor. This allows you to work in a more efficient and organized manner, with complete visibility into your project's file and directory structure.

: **Open Neo-Tree in floating window**: `.` (comma)

: Opens and closes Neo-Tree in a floating window when the ++"."++ is pressed in normal mode.

: **Open Neo-Tree in right window**: `<C-n>`

: Opens and closes Neo-Tree in a right window when pressing ++ctrl+"n"++ in normal mode.

: **Reveal current file in Neo-Tree**: `<leader>fr`

: Opens the current file in Neo-Tree when you press ++space+"f"+"r"++ in normal mode.

These mappings allow you to quickly access Neo-Tree's features and manage the file system in Neovim efficiently. For example, you can open Neo-Tree in a floating window without having to use the standard `:Neotree float` command.

### Bufferline

These mappings help manage buffers more quickly and intuitively, reducing the time spent navigating and managing buffers and allowing you to focus more on editing and development work. This improves productivity and workspace management, making the editor more efficient and easier to use.

: **Select buffer**: `<leader>bp`

: Allows selection of buffers when pressing ++space+"b"+"p"++ in normal mode. Upon typing this key, the buffer type logo is replaced by a character highlighted in red in the bufferline, and typing the corresponding letter brings the focus to the corresponding buffer.

: **Close selected buffer**: `<leader>bc`

: Allows you to close an open buffer when you press ++space+"b"+"c"++ in normal mode. As with selection, the buffer logo is replaced and when the corresponding character is typed, the buffer is closed.

: **Cycles to next buffer**: `<TAB>`

: Switches to the next buffer when the ++tab++ key is pressed in normal mode.

: **Cycles to previous buffer**: `<S-TAB>`

: Switches to the previous buffer when ++shift+tab++ is pressed in normal mode.

These mappings allow you to manage buffers in Neovim quickly and efficiently, without having to use the standard Neovim commands. For example, you can move to the next or previous buffer without having to type `:bnext` or `:bprevious`.

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

These mappings allow you to perform file searches and operations in Neovim quickly and efficiently, without having to use standard Neovim commands. For example, you can open the Telescope window to search for the last modified files without having to type `:Telescope old_files`.

### Diagnostics

The mappings defined in this section represent a set of custom commands for handling diagnostics and problems in one's work environment; they are designed to help users identify and resolve problems in their code more efficiently and quickly, thereby improving productivity and quality of work.

: **Toggle global diagnostics**: `<leader>dt`

: Turns global diagnostics on or off when ++space+"d"+"t"++ is pressed in normal mode.

: **Toggle diagnostics of current buffer**: `<leader>db`

: Activates or deactivates current buffer diagnostics when ++space+"d"+"b"++ is pressed in normal mode.

: **Toggle symbols of current buffer**: `<leader>ds`

: Activates or deactivates the display of current buffer symbols when pressing ++space+"d"+"s"++ in normal mode. This feature applied to Markdown code allows you to navigate between file headers by reducing the time for positioning at the desired location.

These mappings allow you to handle errors and diagnostics in Neovim quickly and efficiently, without having to use standard Neovim commands.

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

: Stops the current session when ++space+"s"+"t"++ is pressed in normal mode.

These mappings allow you to manage sessions in Neovim quickly and efficiently, without having to use the standard Neovim commands. For example, you can load the last saved session without having to type `:SessionLoadLast`.

### Search and Replacement

These customized mappings help simplify search and replacement operations, reducing the time spent searching for text. This allows you to work more efficiently and accurately, with the ability to perform searches and substitutions quickly and individually, and to maintain control over the text and its formatting.

: **Enables/disables Spectre**: `<leader>R`

: Activates or deactivates the search and replace mode provided by *nvim-spectre* when ++space+"R"++ is pressed in normal mode.

: **Current word search**: `<leader>rw`

: Performs a search for the current word in all files in the project when you press ++space+"r"+"w"++ in normal mode.

: **Search current word in file**: `<leader>rp`

: Performs a search for the current word in the current file when pressing ++space+"r"+"p"++ in normal mode.

**Nvim-spectre** allows you to search for text in the current file or in all files in the project, replace the found text with new text, and select the search mode, e.g., search for whole words or search for regular expressions.  
It also offers other advanced features, such as the ability to search multiple files at once, perform directory and subdirectory searches, and exclude files or directories from the search.

### Differences

These mappings are designed to keep track of changes made to files and to manage code versions more efficiently and quickly, improving collaboration and code management. With **diffview.nvim** it is possible to work more transparently and securely, always knowing the changes made to files and the code versions used.

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

These mappings allow you to view the differences between files and buffers in Neovim quickly and efficiently, without having to use the standard Neovim commands. For example, you can open the Diffview window to view the differences between the current file and the previous version without having to type `:DiffviewOpen`.

### Repository Git

These mappings enable operations such as opening git management for the workspace or current buffer, viewing commit history, and accessing git's advanced features; they are designed to help users manage code versioning more efficiently and quickly.

: **Open Neogit**: `<leader>gm`

: Opens Neogit, a user interface for Git, when you press ++space+"g"+"m"++ in normal mode.

: **Open Neogit for current buffer**: `<leader>gM`

: Opens Neogit for the current buffer when pressing ++space+"g"+"M"++ in normal mode.  
This allows you to apply changes to files outside the current workspace without having to leave the session.

: **View commit history**: `<leader>gh`

: Displays the repository's commit history when pressing ++space+"g"+"h"++ in normal mode.

: **View the commit history of the current buffer**: `<leader>gb`

: Displays the commit history of the current buffer when pressing ++space+"g"+"b"++ in normal mode.

Git mapping allows you to manage Git repositories directly within Neovim, view the history of repository commits and buffers, and run Git commands directly from Neovim.

### Terminal

The terminal mappings defined in this section represent a set of custom commands for managing the terminal window in your work environment. With these mappings, users can work in a more integrated way with the terminal, performing system and code management tasks more easily and quickly.

: **Toggle Horizontal terminal**: `<a-t>`

: Allows you to open and close the terminal in horizontal mode. When ++alt+"t"++ is pressed in normal, insertion or terminal mode, the terminal opens or closes in horizontal mode.

: **Toggle vertical terminal**: `<a-v>`

: Allows you to open and close the terminal in vertical mode. When you press ++alt+"v"++ in normal, insertion or terminal mode, the terminal opens or closes in vertical mode.

: **Toggle float terminal**: `<a-f>`

: Allows you to open and close the terminal in float mode. When you press ++alt+"f"++ in normal, insertion or terminal mode, the terminal opens or closes in float mode.

### Logs Management

The mapping defined in this section is used to manage notifications and status messages in your working environment. This mapping allows you to view notifications and status messages from running processes, as well as manage the status information of plugins and extensions.

: **View messages**: `<leader>lg`

: Allows you to view system messages passed to **fidget.nvim**. When you press ++space+"l"+"g"++ in normal mode, a window with editor and plugin status messages opens.

## Conclusions

In conclusion, the mappings presented in this page represent a comprehensive and customized collection of commands and shortcuts to enhance the experience of using the code editor. The `editor.remap()` function has been used extensively to build these mappings, offering great flexibility and customization in defining custom commands.

The `editor.remap()` function allows you to redefine existing commands or create new ones, assigning them specific actions and custom descriptions. This allows you to create a collection of mappings that cover a wide range of functionality, from basic operations such as saving and closing buffers, to more advanced features such as file management, text search and replace, and Git commit management.
