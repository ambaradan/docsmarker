---
title: Marksman
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

# Marksman

## Introduction

Marksman is a language server that integrates with your editor to help you write and maintain your Markdown documents. Using the LSP protocol, it provides completion, workbook navigation, reference searching, name refactoring, diagnostics, and more.

## Installation

The installation of Marksman occurs automatically during the initial setup of Rocksmarker. If it is not available for some reason, you can install it manually with the command:

```txt
:MasonInstall marksman
```

To verify the correct installation of the language server open a markdown file (*.md*) in the editor and type the command `:LspInfo`. This will open a floating buffer containing the information shown below. If it is missing, `:LspInfo` will not detect any clients attached to the buffer.

```txt
 Language client log: /home/your_user/.local/state/nvim/lsp.log
 Detected filetype:   markdown
 
 1 client(s) attached to this buffer: 
 
 Client: marksman (id: 1, bufnr: [1, 14])
  filetypes:       markdown, markdown.mdx
  autostart:       true
  root directory:  /home/your_user/your_dir
  cmd:             /home/your_user/.local/share/nvim/mason/bin/marksman server
```

The message here shows the detection of the markdown file in the buffer, and that there is an attached client (marksman).
It describes the characteristics of the supported file types and that the server started automatically upon detection of those file types. Shown too, is the working directory and the command used for language support.
The `root directory` directive is very important as it indicates the directory that marksman uses for diagnostics, write-assisted linking, and other functionality provided by the server.

This implies that if you open a file contained within the working folder, in this case the `your_dir` folder, it is then checked and supported by marksman at the project level:

```bash
cd /path_to/your_dir
nvim your_file.md
```

If opened from a location outside the root directory, Marksman treats the file without the project's own functionality (such as preview and link management, reference search, and other features):

```bash
nvim ~/path_to/your_dir/your_file.md
```

You can verify the correct implementation of the language server in the status bar where the corresponding server name is displayed, if attached.

![LSP info](../assets/img/lsp-info.png)

## Marksman features

### Assisted navigation

#### Buffer navigation

Marksman provides some useful shortcuts for navigating markdown buffers. You can move between document headers with the combination of two square brackets. With the combination ++"]]"++ you move to the next header, while with the combination ++"[["++ you return to the previous one.

#### Workspace navigation

You can also navigate the internal links in the workspace with the **go to** function, common to all language servers. If the link is internal to the file, as in the case of a TOC (Table of Contents), positioning oneself on the link and recalling the key from the keyboard, automatically positions you in the corresponding section. If the link is external to the file, it opens in a new buffer.  
The function is available through the key ++space+"g"+"d"++.

### Links

#### Auto completion of links

The language server supports, in assisted writing, auto completion of links. The feature is very useful as it accelerates document writing and avoids problems arising from incorrectly written paths.

!!! warning "Absolute or relative path"

    Marksman allows the use of both absolute and relative paths for links, but the use of absolute paths should be carefully evaluated. A *parser* processes the markdown code, and you must check the relative HTML end code of the link for correctness.

    #### Use in Rocky Linux Documentation

    The use of absolute links for writing documents for Rocky Linux is discouraged because the project uses MkDocs for transforming markdown code into static HTML code, and the following caveat is included in the related documentation.

    > Using absolute paths with links is not officially supported. Relative paths are adjusted by MkDocs to ensure they are always relative to the page. Absolute paths are not modified at all. This means that your links using absolute paths might work fine in your local environment but they might break once you deploy them to your production server.

#### Link with absolute path

When entering the text of the link in the square brackets, and then typing the two parentheses, a pop-up will open containing the names of the files in that work area. Selecting any of these provides additional information about the title of the file.  
Selecting the desired file with the ENTER key will automatically place it within the parentheses.

![Absolute path](../assets/img/marksman-absolute-path.png)

#### Link with relative path

Typing the `./` begins the relative path management assistant, and presents a list of the folders and files in the workspace. In this case, however, the position of the relative files location, precedes the name of the target linked file.
This type of link has the advantage of allowing the linking to go back up from the folder's location, using the `../` path, thus allowing multi-folder management of the project.

![Relative path](../assets/img/marksman-relative-path.png)

#### Checking links

Marksman also integrates in its link management, a check for the presence of the corresponding document. This is a great help in management, particularly with document-rich projects such as the documentation on Rocky Linux.
With this, you can avoid typos or distraction errors that are usually difficult to spot.

![Links check](../assets/img/marksman-check-link.png)

!!! tip ""

    The *trouble.nvim* plugin provides extended display of previous screen errors, activated with the shortcut ++space+"t"+"b"++.

#### Preview of links

For links already in the document, it is possible to preview the contents of the file. This feature is particularly useful when reviewing dated documents where you might not always remember the contents of linked files.  
To activate the preview place the cursor on the desired link and type ++"K"++ (uppercase). To close it just move the cursor.

![Link preview](../assets/img/marksman-link-preview.png)

!!! note ""

    The availability of link preview is only for links referring to files in the work area. There are no link previews for web links and for files outside the *workspace*.

#### Rename and restructure links

The language server also allows for workspace-level renaming of headers present in documents. In renaming, marksman also handles any reference links present, ensuring proper navigation.  
For its activation, use the key ++space+"r"+"n"++. Typing this opens the message `==:New Name==` in the status bar, followed by the text of the header at the cursor's location. Changing the text changes the header and all reference links for that header.

!!! info "Rocky Documentation"

    Reference links in documents written for Rocky Linux should be avoided. This is because they are not compatible with the development environment used. The multilingual nature of the site and the related processing by the Crowdin translation engine does not allow the translation of these types of links.

### Code Action

A CodeAction represents a change or command with the possibility of running the change on the code. For example, for problem solving or restructuring. Marksman has a code action for managing TOC (table of contents). To create it, just place the cursor at the desired location and type the key ++space+"c"+"a"++ and the following pop-up will appear:

![Marksman TOC create](../assets/img/marksman-create-toc.png)

Selecting the code action creates the reference link table, which then allows editing and managing with the restructuring function described earlier (++space+"r"+"n"++).  
Here is an example using the TOC of the README for the Rocksmarker project repository:

![Marksman TOC code](../assets/img/marksman-toc-code.png)

If the TOC is already present, the code action allows updating automatically, based on changes made to the document:

![Marksman TOC edit](../assets/img/marksman-toc.png)

!!! info "Rocky Documentation"

    This feature is also not suitable for documents written for Rocky Linux. The *mkdocs-material* plugin, handles document reference navigation automatically in MkDocs.

## Conclusion

Although not strictly necessary, this language server can become an excellent companion in writing documentation for Rocky Linux.
Its use saves time in building page structure and avoids trivial errors such as typos.
