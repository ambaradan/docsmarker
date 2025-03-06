---
title: Autocommands
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

## Introduction

Automated commands (*autocmds*) in Neovim are a mechanism for defining custom behaviors and automated actions that are triggered in response to specific events within the editor. These event-driven commands allow you to create highly personalized and dynamic editing experiences by executing predefined functions or actions when particular conditions are met; they provide a way to extend and customize Neovim's functionality beyond the default settings.

They can be used to automatically perform tasks such as formatting code, setting specific indentation rules, applying syntax highlighting or performing complex operations when files are opened, saved or edited. This flexibility enables the creation of sophisticated workflows that adapt to different file types, project structures, or personal preferences.

The auto commands are implemented using Lua's event handling system in Neovim, allowing for more concise and performant configuration than traditional Vimscript. They can be concatenated, conditionally executed, and integrated with other Neovim APIs, providing a robust mechanism for creating complex and intelligent editing behaviors that adapt to different programming languages, project types, and individual workflow requirements.

### Syntax

The basic structure of an *autocommand* is designed to be flexible and intuitive, allowing the definition of custom behaviors triggered by specific events. The fundamental syntax revolves around the occurrence of an **event**, an optional **pattern** for matching *files* or *buffers*, and the **action** to be performed when the event occurs.

The basic components of an autocommand typically include the event type, which represents the specific time and action that triggers the command.  
Events can range from file-related actions, such as opening, saving, or creating a new buffer, to more complex interactions, such as cursor movements, mode changes, or plugin-specific triggers. The event is the primary mechanism for detecting when a particular action is to be initiated.

The action of an autocommand defines the command or function to be executed when the specified event occurs. In Neovim's Lua configuration, this typically involves calling a function, setting specific buffer options, or performing complex transformations. The action can be a simple one-line command or a more elaborate function that implements sophisticated logic based on the current editing context.

The basic syntax is as follows:

```lua
-- Basic Autocmd Template
vim.api.nvim_create_autocmd({event}, {
    desc = "Description of autocmd purpose",
    pattern = {pattern},
    group = vim.api.nvim_create_augroup("group_name", { clear = true }),
    callback = function(args)
        -- Your code here
    end
})
```

The *vim.api.nvim_create_autocmd()* function is provided by the Neovim Lua API to create custom autocommands. This function provides a clean approach to defining event-driven behavior within the editor. It requires two main arguments: the **event type** and a **configuration table** that specifies additional parameters.

The first argument represents the event or array of events that will trigger the autocommand. These events can be specific actions such as "*BufEnter*", "*FileType*", "*InsertLeave*" or multiple events passed as a table. This flexibility allows the creation of complex behaviors triggered by multiple events that respond to various editor interactions.

The *configuration table* contains optional keys. The "*pattern*" key allows you to interact with specific file types, file extensions, or buffer characteristics. This allows you to control precisely when the autocommand should be executed. For example, an autocommand can be set to be applied only to terminal buffers or specific directory structures.

The "*callback*" function is where the actual logic of the autocommand is implemented. This function contains the code that will be executed when the specified event occurs. It provides a sandbox for implementing custom behaviors, such as formatting, setting specific buffer options, or performing complex transformations.

The other optional keys are used to organize and manage the autocommands ("*group*") and to provide a description for the specific autocommand ("*desc*"), thereby improving the readability and maintainability of the code.

### Augroups

A group (*autogroup*) is a mechanism for organizing and managing related autocommands within the editor scripting environment. It is used to group multiple event-driven commands under a single collection, allowing the creation of more modular and manageable editor behaviors. Using the **clear = true** option, autogroups ensure that existing commands are removed before adding new ones, avoiding potential conflicts or duplicate events.

The basic structure is as follows:

```lua
-- Create the augroup
local augroup = vim.api.nvim_create_augroup("MyCustomGroup", { clear = true })
-- Define an autocommand within the group
vim.api.nvim_create_autocmd("{event}", {
    group = augroup,
    pattern = "{pattern}",
    callback = function()
        -- Your code here
    end
})
```

The code snippet shows the creation of an autogroup, with the subsequent creation of an autocommand. Using *vim.api.nvim_create_augroup()*, a new autogroup named "*MyCustomGroup*" is created with the option *clear = true*, which ensures the removal of any existing commands in the group before creating new ones.

## Rocksmarker Commands

The configuration of autocommands in *Rocksmarker* are defined in the `lua/commands.lua` file and starts with the group configuration:

```lua
local augroup = vim.api.nvim_create_augroup(“RocksmarkerSet”, { clear = true })
```

The line of code is a Lua instruction used to define a local group of autocommands. This group, called "*RocksmarkerSet*" is used to organize related autocommands into a cohesive unit for ease of management. By including the option *{ clear = true }*, the code ensures that all existing autocommands within this group are removed before adding new ones, avoiding potential conflicts or duplication.

### `checktime`

```lua hl_lines="1 5 6"
vim.api.nvim_create_autocmd({ "FocusGained", "TermClose", "TermLeave" }, {
 group = augroup,
 desc = "Reload files when focus is regained or terminal interactions occur",
 callback = function()
  if vim.o.buftype ~= "c" then
   vim.cmd("checktime")
  end
 end,
})
```

This autocommand is triggered by three specific events: when the window gets focus, when a terminal closes, and when the user leaves a terminal session. It is part of the augroup of autocommands, defined earlier, which organizes related functionality.  
The purpose of this autocommand is to reload the files in the buffer whenever the user regains focus or interacts with a terminal. The callback function checks if the current buffer type is not a command line ("*c*"); if it is, it executes the *checktime* command, which checks if the file has been externally modified and reloads it if necessary. This feature ensures that the most up-to-date version of a file is always displayed, particularly after terminal interactions that may affect the contents of the file.

### `close_with_q`

```lua hl_lines="1 4 16 17"
vim.api.nvim_create_autocmd("FileType", {
 group = augroup,
 desc = "Allow closing specific utility buffers with 'q' key",
 pattern = {
  "PlenaryTestPopup",
  "help",
  "lspinfo",
  "man",
  "notify",
  "qf",
  "startuptime",
  "checkhealth",
  "spectre_panel",
 },
 callback = function(event)
  vim.bo[event.buf].buflisted = false
  vim.keymap.set("n", "q", "<cmd>close<cr>", { buffer = event.buf, silent = true })
 end,
})
```

This autocommand improves the user experience by creating consistent behavior for closing specific utility buffers using the ++"q"++ key. The autocommand is triggered for a predefined list of buffer types, including help screens, quick fix windows, notifications, LSP information panels, and other utility buffers. When these buffer types are loaded, the script automatically sets up two key behaviors: first, it marks the buffer as hidden (preventing it from appearing in buffer lists), and second, it maps the **q** key to immediately close the current buffer. This provides a method for quickly closing utility windows in different Neovim contexts, improving navigation and reducing the need for multiple key combinations to exit specialized buffers.

### `highlight_yank`

```lua hl_lines="1 5"
vim.api.nvim_create_autocmd("TextYankPost", {
 group = augroup,
 desc = "Highlight text after yanking to provide visual feedback",
 callback = function()
  vim.highlight.on_yank()
 end,
})
```

This autocommand is intended to enhance the text copying experience by providing visual feedback during operations. The autocommand is triggered by the "*TextYankPost*" event, which occurs immediately after copying a text selection.  
By calling the *vim.highlight.on_yank()* function within the callback, the script temporarily highlights the newly copied segment of text, creating a brief visual indication of the copying action. This implementation provides an effective method for confirming text selection, helping during editing operations and reducing potential errors by making text manipulation more user-friendly.

### `vertical_help`

```lua hl_lines="1 4 6-8"
vim.api.nvim_create_autocmd("FileType", {
 group = augroup,
 desc = "Open help documents in a vertical split and equalize window sizes",
 pattern = "help",
 callback = function()
  vim.bo.bufhidden = "unload"
  vim.cmd.wincmd("L")
  vim.cmd.wincmd("=")
 end,
})
```

This autocommand is designed to enhance the experience of viewing help documents by automatically adjusting the window layout when opening help files.  
Triggered by the "*FileType*" event with a specific pattern for "*help*" buffers, it implements three key behaviors. First, it sets the buffer's **bufhidden** option to "*unload*" which helps to efficiently manage buffer memory. Second, it uses *vim.cmd.wincmd("L")* to move the help window to the far right of the editor, creating a split vertical layout. Finally, use *vim.cmd.wincmd("=")* to equalize the size of all open windows, ensuring a balanced and aesthetically pleasing display. This code allows for an optimized display of the help documentation for better readability and navigation of its contents.

### `term_spell_off`

```lua hl_lines="1 5"
vim.api.nvim_create_autocmd({ "TermOpen" }, {
 group = augroup,
 desc = "Disable spell checking when opening terminal buffers",
 callback = function()
  vim.wo.spell = false
 end,
})
```

This autocommand is designed to improve the terminal buffer experience by automatically disabling the spell check feature when the terminal is opened. Triggered by the "*TermOpen*" event, the autocommand focuses on setting the local window spelling option (*vim.wo.spell*) to **false**. This implementation ensures that when you open a terminal buffer within Neovim, you are not distracted by highlighting or spell-checking hints, which are typically irrelevant in command-line interfaces.

## Conclusions

Autocommands in Neovim are a powerful feature that allows you to customize Neovim's behavior by automatically executing functions or commands when specific events occur. They can be used to automate various tasks, such as formatting code, setting file types, or resizing windows, making Neovim's configuration more efficient and tailored to your needs.
