---
title: Autocmds
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

## Introduction

Neovim offers a wide range of features to customize and automate your workflow. Among these, autocmds (short for “auto-commands”) are one of the most powerful and versatile features. In this chapter, we will explore the features provided by Rocksmarker's autocmds and how they are used to improve productivity and the editor experience.

### What is an Autocmd?

An autocmd is a command that is executed automatically when a certain event occurs within Neovim. Events can range from actions such as opening a file, editing a buffer, closing a window, to more specific events such as inserting a character or pressing a key.

### Autocmds functionality

Autocmds in Neovim offer a range of features that can be used to customize and automate your workflow. Some of the most important features include:

: **Automatic execution of commands**

: Autocmds can be used to execute commands automatically when a specific event occurs.

: **Interface customization**

: Autocmds can be used to customize the Neovim interface, for example by changing the buffer display, window settings, etc.

: **File and buffer management**

: Autocmds can be used to manage files and buffers, for example by automatically loading a file when a buffer is opened, or saving a file when a buffer is closed.

: **Integration with other tools**

: Autocmds can be used to integrate Neovim with other tools and applications, for example by executing external commands or interacting with other editors.

### Benefits of Autocmds

Autocmds offer a number of advantages over other methods of customization and automation in Neovim.

: **Flexibility**

: Autocmds can be used to execute a wide range of commands and actions, making them extremely flexible and customizable.

: ***Automation**

: Autocmds can be used to automate many repetitive tasks, freeing up the user's time to focus on more important activities.

: **Integration**

: Autocmds can be used to integrate Neovim with other tools and applications, improving productivity and efficiency.

## Autocmds in Rocksmarker

The `lua/commands.lua` file plays an important role in configuring the Rocksmarker editing environment. This file contains a series of commands and settings that improve the user experience by automating various operations and customizing the editor's behavior.

The *commands.lua* file contains several sections covering different aspects of Neovim configuration. The main areas of interest are:

: **Autocmd groups**

: Vengono definiti gruppi di autocmd per gestire eventi specifici, come il riavvio dell'editor o la chiusura di una finestra del terminale.

: **File management**

: Rules are defined for restarting files when the editor regains focus or when interactions with the terminal occur.

: **Closing specific files**

: Shortcuts are defined to quickly close certain types of files, such as help buffers or notification windows.

: **Text highlighting**

: Text highlighting is activated after copying to provide visual feedback to the user.

: **Help window management**

: Rules are defined for opening help pages in a vertical window and equalizing window sizes.

### RocksmarkerSet group

Creating an autocmd group with a unique name and the ability to empty it offers great flexibility and ease of management for automatic actions in Neovim.

```lua
local augroup = vim.api.nvim_create_augroup("RocksmarkerSet", { clear = true })
```

This line of code creates a new autocmd group called **RocksmarkerSet** and empties it if it already exists, so that new autocmds can be added without conflicting with existing ones.  
The autocmd group is then assigned to the **augroup** variable, which can be used later to add new autocmds to the group.
This allows you to maintain an organized and easy-to-manage structure for automatic actions within Neovim.

#### Autocmd for restarting files

This autocmd is particularly useful for ensuring that the contents of files opened in Neovim are always up to date, even when external changes are made. This helps prevent situations where the user might be working on an outdated version of a file, reducing the risk of data loss or overwriting changes.

```lua title="Reload files"
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

This autocmd is triggered by three different events:

: `FocusGained`

: When the Neovim editor regains focus after being minimized or covered by another window.

: `TermClose`

: When a terminal window within Neovim is closed.

: `TermLeave`

: When the user exits terminal mode within Neovim.

When one of these events occurs, the autocmd executes the defined *callback* function. Within this function, it checks whether the buffer type (**buftype**) is not “**c**,” which indicates a command buffer. If the condition is true, the autocmd executes the **checktime** command.

The *checktime* command checks whether the file opened in Neovim has been modified externally (e.g., by another program) and, if so, restarts the file within Neovim to reflect the latest changes.

#### Quickly close certain file types

The autocmd for quickly closing certain types of files is a useful feature that simplifies file and buffer management in Neovim, improving the user experience and increasing productivity.

```lua title="Close some filetypes"
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

This autocmd is activated for the following file types:

- PlenaryTestPopup
- help
- lspinfo
- man
- notify
- qf
- startuptime
- checkhealth
- spectre_panel

When you open a file of one of the types listed above, autocmd executes the defined callback function. Within this function, two operations are performed:

: `vim.bo[event.buf].buflisted = false`

: This command sets the **buflisted** option of the buffer to *false*, which means that the buffer will not be listed in the list of open buffers in Neovim.

: `vim.keymap.set("n", "q", "<cmd>close<cr>", { buffer = event.buf, silent = true })`

: This command defines a key mapping for the current buffer. In normal mode (“n”), pressing the ++“q”++ key will execute the `close` command, which will close the *buffer*. The *silent = true* option ensures that the command is executed without displaying status messages.

#### Highlighting text after copying

The autocmd for highlighting text after copying is a feature that helps you immediately view the copied text and better understand the context in which to use it.

```lua title="Highlight on yank"
vim.api.nvim_create_autocmd("TextYankPost", {
 group = augroup,
 desc = "Highlight text after yanking to provide visual feedback",
 callback = function()
  vim.highlight.on_yank()
 end,
})
```

This autocmd is activated when the **TextYankPost** event occurs, which happens after text has been copied to the clipboard.

When the *TextYankPost* event occurs, the autocmd executes the defined callback function. Within this function, the `vim.highlight.on_yank()` function is called, which activates the highlighting of the copied text.

The copied text is highlighted for a short period of time after copying.

#### Vertical help buffer

This autocmd allows you to view help pages in a vertical window and equalize the size of the windows. Opening help pages in a vertical window instead of the default horizontal window in Neovim allows you to view help information in a more ergonomic way.

```lua title="Vertical help"
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

This autocmd is triggered when you open a file of type **help**, which is the file type used for help pages in Neovim.

When a *help* file is opened, the autocmd executes the defined *callback* function. Within this function, three operations are performed:

: `vim.bo.bufhidden = "unload"`

: This command sets the **bufhidden** option of the buffer to “*unload*,” which means that the buffer will be closed when it is no longer needed.

: `vim.cmd.wincmd("L")`

: This command executes the **wincmd** command with the argument “**L**”, which means “go left” and opens the help window to the right of the current window.

: `vim.cmd.wincmd("=")`

: This command executes the **wincmd** command with the argument “**=**”, which means “equalize window sizes” and ensures that windows are the same size.

Vertical help helps you work more efficiently and productively, as you can easily view help information and navigate between windows without having to leave the current window.

## Conclusion

The *commands.lua* file is an important element in customizing the Neovim editing environment. By defining various autocmds, this file allows you to automate repetitive tasks, improve productivity, and simplify your workflow.  
Each of the features included in the file is designed to improve the user experience and increase productivity, allowing you to work more efficiently and with greater focus.
