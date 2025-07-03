---
title: Install
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

## Introduction

Rocksmarker is an experimental IDE project for writing documentation in Markdown that takes advantage of the new **rocks.nvim** plugin manager for Neovim.  
To take full advantage of its potential and ensure proper installation, it is essential to properly prepare the underlying Linux environment. This chapter outlines the necessary steps, from configuring the repositories to installing Neovim and a specific version of Lua, which is essential for the proper functioning of Rocksmarker.

### Prerequisites

- A Linux distribution installed and configured as a desktop version. This guide uses Rocky Linux and consequently should work correctly on all RHEL (Red Hat Enterprise Linux) derivatives. For installation on other distributions refer to the relevant documentation.
- Practice in executing commands from the terminal.
- Ability to run some commands as root user or with administrator permissions.

### Prerequisites for Neovim, Lua and Rocksmarker

The project requires some specific dependencies to run, in particular, you will need Lua version ==5.1==. Versions provided by Rocky Linux are not supported.  
Version 5.1 ensures the proper functioning of *rocks.nvim*, which handles plugin management, while also ensuring full compatibility with the version used by Neovim.

From Neovim docs:

> The Lua 5.1 script engine is built-in and always available.

Also required are some additional packages, such as the *ninja-build* package, provided by the [CRB repository](https://wiki.rockylinux.org/rocky/repo/#notes-on-crb) (CodeReady Linux Builder). The repository provides common tools for code development and on Rocky Linux you can enable it with the following commands:

```bash
sudo dnf install -y epel-release yum-utils
sudo dnf config-manager --set-enabled crb
```

Once you set up the sources, it is necessary to install some additional packages required for both building *Neovim* and installing the required Lua version. To install them on a Rocky Linux distribution type:

```bash
dnf install npm ncurses readline-devel icu ninja-build cmake gcc make unzip gettext curl glibc-gconv-extra tar git
```

Once finished, you can move on to building the structure, which involves, as a first step, installing the *Neovim* editor.

## Installation of Neovim

*Neovim* is a modern, powerful and highly customizable text editor that provides a familiar experience for Vim users.  
While maintaining the philosophy of Vim, it simultaneously introduces improvements and features with the goal of providing a similar user experience but on a more modern codebase.  
Its improvements include:

- **Improved stability**: The refactoring of the codebase to improve stability and performance.
- **Improved plugin system**: Neovim enables simplified plugin management by making it easier to extend its functionality.
- **Modern features**: Supports features such as asynchronous operations and better integration with other applications.

!!! notes "Compiled version"

    For proper execution of Rocksmarker, Neovim must be version ==0.11.0== or higher, so the recommendation is to use the compiled version. The version provided by *EPEL* is outdated (0.8.0) and is not compatible with *rocks.nvim* and the plugins used.  
    More information can be found in the instructions in the [Quick Start](https://github.com/neovim/neovim/blob/master/BUILD.md) section of the Neovim repository.

Compiling Neovim from source presents no particular problems and, when you have met the above requirements, it is easy to perform.

### Download the sources

The development of this project is on *GitHub* and to retrieve the sources you need to clone the repository locally. Then take yourself to the chosen location of your file system and download them with:

```bash
git clone https://github.com/neovim/neovim
```

The command creates a `neovim` folder and downloads all the files inside it. When you have finished the clone, take yourself to the newly created folder and switch to the ==stable== version with the commands:

```bash
cd neovim/
git checkout stable
```

!!! note ""

    Using the ==git checkout stable== command to place *git* in the stable branch before compilation ensures that the stable (recommended) version is used. If you omit this, the compilation will be performed with the development branch (currently 0.12).

### Compilation and installation of Neovim

One command is sufficient for its compilation:

```bash
make CMAKE_BUILD_TYPE=RelWithDebInfo
```

The command calls *make* which takes care of the entire process and passes the **CMAKE_BUILD_TYPE** flag that specifies the type of compilation.  
In particular, the **RelwithDebInfo** compilation produces fully optimized code, but it also builds the program database and inserts debug line information useful for any editor errors.

After the compilation finishes, it is possible to verify the correct running of the new build even without installing it. Neovim does not require external components to run, and consequently it is sufficient to call up the newly compiled executable to thoroughly test the new version of the editor before replacing the currently used version.

```bash
./neovim/build/bin/nvim
```

Its installation, when verified to be working properly, is always performed by *make*. The files, as with Lua installation, copy into `/usr/local`, and again, you must run the command as a *root* user or with administrator permissions.

```bash
sudo make install
```

The installation makes available in the system the new command ==nvim== for opening the editor in the terminal, which you can also use to check the installed version:

```bash
nvim --version
NVIM v0.10.4
Build type: RelWithDebInfo
LuaJIT 2.1.1713484068
Run "nvim -V1 -v" for more info
```

!!! tip "Removing Neovim"

    For its removal, a CMake target is provided that removes all files installed by ==make install==

    ```bash
    sudo cmake --build build/ --target uninstall
    ```

    It is therefore advisable to keep the folder with the sources for eventual removal.

With the editor ready to use, you can move on to installing the version of *Lua* corresponding to the one provided by *Neovim*.

## Installing Lua 5.1
  
The plugin manager chosen to configure additional Neovim plugins (**rocks.nvim**), requires the installation of *Lua 5.1* to function properly. It must also be the default version for that user space. You must also link the 5.1 *headers* files to those on the system.  

!!! notes "Coexistence of multiple versions"

    One of the features of Lua, resulting from its development as an "embedded" language, is that it does not require external dependencies. As a result, different versions can coexist on the same system without conflicting.  
    For example, a desktop installation of Rocky Linux 9 provides version 5.4.4, used by the various applications that require it, even though the default version is 5.1.

### Download the required version

Version 5.1 is available on the official Lua website on the [download](https://www.lua.org/download.html) page. The latest updated version is ==5.1.5== that you can download with the command:

```bash
curl -O https://www.lua.org/ftp/lua-5.1.5.tar.gz
```

Once downloaded verify the integrity of the downloaded package with the *sha256* utility by comparing the result of the following command with the *checksum* provided by the download site:

```bash
sha256sum lua-5.1.5.tar.gz 
2640fc56a795f29d28ef15e13c34a47e223960b0240e8cb0a82d9b0738695333  lua-5.1.5.tar.gz
```

After verifying the integrity of the download, extract the compressed archive and change to that directory:

```bash
tar xzf lua-5.1.5.tar.gz
cd lua-5.1.5
```

### Compilation and installation of Lua

Lua installs itself as a system installation (in `/usr/local/`). This simplifies subsequent configuration of *headers* files, and administrator permissions are consequently needed at this stage.

For proper compilation you need the additional *readline-devel* package available in the official Rocky Linux repositories. The package should already be available if you installed the packages mentioned in the prerequisites.

!!! note ""

    The package takes on this name in distributions derived from RHEL, but in others it is identified differently. Debian, for example, identifies it as *libreadline-dev*.

Build the version with:

```bash
make linux test
```

The command performs a requirements check and proceeds to compile the binaries. If everything finishes without errors, you will see the following message printed to the terminal:

> Hello world, from Lua 5.1!

Use the *make* system utility to install it. You must run the command as a *root* user, or with administrator privileges:

```bash
sudo make install
```

The installation copies the 5.1.5 version files to the `/usr/local/` path, thus keeping them separate from the system version files installed by default in `/usr/`.  
Here is a list of the files installed:

- **lua** - **luac** -> `/usr/local/bin/`
- **lua.h** - **luaconf.h** - **lualib.h** - **lauxlib.h** - **lua.hpp** -> `/usr/local/include/`
- **liblua.a** -> `/usr/local/lib/`

!!! tip ""

    The compilation does not provide a utility for removal of the installation. You can remove everything manually by deleting the files listed above.

### Version setting

If your system is a desktop version, a default version of Lua is most likely already installed. However, it is unlikely that this version matches the one you require, so you need to set the default version to point to the correct version.

!!! notes "Pre-installed version"

    The version installed on the system will surely be later than 5.1, but in this regard it should be pointed out that versions after 5.1 are not considered stable as they are not compatible with each other.  
    To check the installed version, type:

    ```bash
    lua -v
    Lua 5.4.4  Copyright (C) 1994-2022 Lua.org, PUC-Rio
    ```

The entire project bases itself on the stable version of Lua, used by both Neovim and rocks.nvim, and it is imperative that the default version for the user profile is 5.1.  
To meet this requirement add an *alias* in *.bashrc* to tell the system the version to use as the default for that user space.
Open your `.bashrc` in an editor and add the following string:

```bash
alias lua=/usr/local/bin/lua
```

Save the file and run the *source* to reread the configuration with:

```bash
. ~/.bashrc
```

Once you re-read the file verify the version change with:

```bash
lua -v
Lua 5.1.5  Copyright (C) 1994-2012 Lua.org, PUC-Rio
```

Every time you request the executable, the system will now use the required version instead of the system version.

### Add header files

Installing the required version is not sufficient for the configuration to work properly. The *rocks.nvim* plugin manager needs Lua's *headers* files to compile its version of *luarocks*.  
You must then link the required library, **lua.h**, found in `/usr/local/include/` in the *headers* file search path (`/usr/include/`).

!!! warning ""

    The lack of this file does not allow the initialization script to compile *luarocks* which ends with an error aborting the entire process.

For its linking you use one of the standard *luarocks* `/usr/include/lua/<number_version>` search paths, linked to the folder with version 5.1 *headers* files in `/usr/local/include/`.

Again, you need to be *root* or elevate your permissions to administrator with `sudo` to implement this. Then change to the `/usr/include` directory:

```bash
cd /usr/include/
```

Create the `lua` folder and change to that directory. The `/usr/include/lua/` directory is not present on a Rocky Linux system, so you must create it.

```bash
sudo mkdir lua && cd lua
```

Then create the symbolic link to the `/usr/local/include/` folder.

```bash
sudo ln -s /usr/local/include/ 5.1
```

The command links the folder where you copied the *headers* files during the installation of Lua 5.1, to a folder named `5.1` created by the command itself.  
The folder naming is arbitrary but choosing to use the version number allows mnemonic recall to its contents and more importantly meets the requirements for *luarocks* path finding.

For verification, a listing of the `5.1` folder and the files contained in `/usr/local/include/` should show:

```bash
ls -l /usr/include/lua/5.1/
total 52
-rw-r--r--. 1 root root  5777 27 dic  2007 lauxlib.h
-rw-r--r--. 1 root root 22299 11 feb  2008 luaconf.h
-rw-r--r--. 1 root root 11688 13 gen  2012 lua.h
-rw-r--r--. 1 root root   191 23 dic  2004 lua.hpp
-rw-r--r--. 1 root root  1026 27 dic  2007 lualib.h
```

With this last step, the installation environment is complete, you have all the necessary requirements, and you can move on to installing the editor.

## Download the configuration

You can use the configuration, although still under development, daily to write and edit documentation written in Markdown.  If you want to use it as the default configuration, install it in the `.config/nvim` path (see "Main editor").  
If you already have a Neovim configuration on their system, there is the option of using *Rocksmarker* as a secondary editor, thus allowing you to continue using your existing configuration to develop your other projects.  
You can also use this method to try *Rocksmarker*, completely independently, to evaluate whether it can be a useful tool for your daily work (see "Secondary editor").

### Main editor

To install the configuration in the default Neovim location, clone the GitHub repository to the configurations folder `~/.config/` with the command:

```bash
git clone https://github.com/ambaradan/rocksmarker.git ~/.config/nvim
```

Once finished, just run the standard Neovim command to start the installation:

```bash
nvim
```

### Secondary editor

To test or use the configuration as a secondary configuration, use Neovim's variable *NVIM_APPNAME*. Use of this variable allows Neovim to pass an arbitrary name for searching the configuration files in `~/.config/` and for the subsequent creation of the shared file folder in `~/.local/share/` and the cache in `~/.cache/`.  
To set *Rocksmarker* as a secondary editor:

```bash
git clone https://github.com/ambaradan/rocksmarker.git ~/.config/rocksmarker/
```

Once the clone is complete, start Neovim with the following command to begin the installation:

```bash
NVIM_APPNAME=rocksmarker nvim
```

!!! warning "Subsequent starts"

    If you choose this method, you must use this command for all subsequent starts. Otherwise, Neovim will start using the default `~/.config/nvim` folder. To avoid typing the entire command each time, create an *alias*.

    ```bash
    alias rocksmarker="NVIM_APPNAME=rocksmarker nvim"
    ```

## Installing the configuration

When you start Neovim with either of the two methods described, it will begin the installation process managed by a *bootstrap* script that checks for the lack of the *rocks.nvim* plugin and proceeded to install it.  
The process first installs *luarocks* required as a dependency of the plugin and then the plugin itself. At the end of this process, if everything worked correctly, it will prompt you to press ++"ENTER"++ to continue.

```text title="rocks.nvim bootstrap"
Downloading luarocks...
Configuring luarocks...                                                                                                            
Installing luarocks...                                                                                                             
Installing rocks.nvim...                                                                                                           
rocks.nvim installed successfully!                                                                                                 
Press ENTER or type command to continue 
```

The second step is to synchronize all configured plugins; synchronization installs the plugins in the shared files folder in the path `.local/share/nvim/rocks/lib/luarocks/rocks-5.1/`.  
Responding with ++"Y"++ will begin the installation process of all configured plugins.

```text
rocks.nvim: The following plugins were not found:                                                                                    
trouble.nvim, mason.nvim, mason-lspconfig.nvim, nvim-lspconfig, nvim-cmp,
mason-tool-installer.nvim, luasnip, markdown.nvim, markview.nvim,
markdown-table-mode.nvim, zen-mode.nvim, conform.nvim, nvim-cokeline,
gitsigns.nvim, bamboo.nvim, telescope.nvim, persisted.nvim, toggleterm.nvim,
neo-tree.nvim, neogit, nvim-spectre, indent-blankline.nvim, nvim-autopairs,
nvim-highlight-colors, yanky.nvim, cmp-buffer, cmp-nvim-lsp, cmp-path,
cmp_luasnip, diffview.nvim, friendly-snippets, rocks-treesitter.nvim,
telescope-cmdline.nvim, telescope-file-browser.nvim, telescope-frecency.nvim,
telescope-ui-select.nvim, tree-sitter-bash, rainbow-delimiters.nvim,
tree-sitter-cli, rocks-config.nvim, tree-sitter-css, rocks-edit.nvim,
tree-sitter-git_config, tree-sitter-git_rebase, tree-sitter-gitattributes,
tree-sitter-gitcommit, tree-sitter-gitignore, tree-sitter-html, tree-sitter-ini,
tree-sitter-json, tree-sitter-jsonc, tree-sitter-lua, tree-sitter-luadoc,
tree-sitter-markdown, tree-sitter-markdown_inline, tree-sitter-toml,
tree-sitter-vim, tree-sitter-vimdoc, tree-sitter-yaml, which-key.nvim,
fidget.nvim, feline.nvim, nvim-lint, rocks-lazy.nvim, rocks-git.nvim,
nvim-web-devicons.                  
                                                                                                                                     
Run 'Rocks sync'?                                                                                                                    
[Y]es, (N)o:  
```

Once the installation of the plugins finish, close the editor and reopen it to give Neovim a chance to load the new configurations. On the second startup, the *mason-lspconfig* and *mason-tool-installer* plugins , the *language servers* (LSP), *linters*, and *formatters*, necessary for the correct functioning of the editor, install. When the installation of the language servers finish, the editor is ready to use.

## Conclusions

Rocksmarker is a custom configuration of Neovim and integrates all of its features. It also provides many additional features for file management, git repositories, diagnostics, and more.  
For an overview of the shortcuts that activate the functions, consult the [mappings](./mappings.md) page.  
Alternatively, you can list all available commands with the ++space++ key that activates the command menu. Letters marked with a **+** provide additional selections. Selecting the corresponding letter switches to the context menu and returns to the main menu with ++back++ while ++esc++ closes the menu without executing any commands.
