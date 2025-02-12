---
title: Markdownlint
author: Steven Spencer
contributors: Franco Colussi
tags:
    - neovim
    - editor
    - markdown
---

## Linters and markdownlint defined

A linter is a tool that examines code for errors, bugs, and stylistic issues. Rocksmarker is not just an editor for markdown, but a linter as well. It uses a variety of tools as its linting mechanism. Markdownlint is a project dedicated to providing rules based on proper markdown formatting. Rocksmarker implements Markdownlint through the `.markdownlint.yaml` file explained later on.

## Introduction

Neovim is a great editor, but the real power of the project comes from the wealth of plugins and additions that makes it possible to mold the editor into what works best for you. This is why Rocksmarker makes such a good editor for markdown, because the author of the project focuses on this goal. The markdownlint rules are just one of these tools. For someone with a little `vim` or Neovim experience already, Rocksmarker makes it possible to start creating or editing markdown code as soon as installation completes. Even without this experience, it takes a minimal length of time to get the basics down. With Markdownlint, the user is free of debugging document formatting on their own.

## The `.markdownlint.yaml` file

Rocksmarker includes the rules file for linting, `.markdownlint.yaml`, in the root of the project. You can find this file in other projects too, although the rules in it might be different. The Rocky Linux documentation includes a `.markdownlint.yaml` file with contents very similar to Rocksmarker. The rules in use at the time of this writing are from the latest version (v0.37.4) of [David Anson's rule set](https://github.com/DavidAnson/markdownlint).

### Rules explanation

Rocksmarker keeps the defaults for most of the rules in the `.markdownlint.yaml`. There are a few exceptions.

| Rule  | Explanation                                                                      | Value                           |
|-------|----------------------------------------------------------------------------------|---------------------------------|
| MD001 | Enforces heading incrementing. Skipping from h1 to h3 is not allowed.            | default                         |
| MD003 | Enforces heading style. Rocky Linux only supports ATX style headings anyway.     | default                         |
| MD004 | Unordered list style must be consistent. (use only - or * but not both)          | default                         |
| MD005 | Inconsistent list item indentation. Enforces proper indentation of all lists.    | default                         |
| MD007 | Unordered list indentation. Spaces for indentation.                              | spaces for indent modified to 4 |
| MD009 | Trailing spaces. Sets the allowed value for trailing spaces.                     | default                         |
| MD010 | Enforces no hard tabs or white space. Ensures spaces only for indentation.       | default                         |
| MD011 | Enforces the proper formatting of links.                                         | default                         |
| MD012 | Only allow a single blank line between elements.                                 | default                         |
| MD013 | Line length rules.                                                               | disabled                        |
| MD014 | No preceding $ in code with no output displayed.                                 | default                         |
| MD018 | Enforces the space after the hash on an ATX heading.                             | default                         |
| MD019 | Enforces only one space after the hash on an ATX heading.                        | default                         |
| MD020 | Enforces spaces after closed ATX heading before the hash. Not used.              | default                         |
| MD021 | For closed ATX headings. Not used.                                               | default                         |
| MD022 | Enforces surrounding headings with a blank lines.                                | default                         |
| MD023 | Enforces headings starting at the beginning of a line. (no indentation)          | default                         |
| MD024 | No duplicate headings.                                                           | default                         |
| MD025 | Enforces only one top level heading (h1) in a document.                          | default                         |
| MD026 | No trailing punctuation in heading.                                              | default                         |
| MD027 | Enforces only a single space after the block quote symbol (`>`).                 | default                         |
| MD028 | No blank lines inside of a block quote.                                          | default                         |
| MD029 | Ordered list prefix.                                                             | default                         |
| MD030 | Enforces single space after list markers.                                        | default                         |
| MD031 | Enforces blank lines around code blocks.                                         | default                         |
| MD032 | Enforces a blank line around lists of any kind.                                  | default                         |
| MD033 | No inline HTML. Rocky Linux shows errors when using HTML elements.               | default                         |
| MD034 | No bare URL. To use a URL outside of a link, you must surround it with (`<>`)    | default                         |
| MD035 | Horizontal rule style must be consistent.                                        | default                         |
| MD036 | Do not use emphasis to replace a heading with (`**my text**`)                    | default                         |
| MD037 | No space in emphasis. When using bold or italic format without spaces (`*this*`) | default                         |
| MD038 | No spaces in code (single backtick without spaces)                               | default                         |
| MD039 | No spaces inside link text.                                                      | default                         |
| MD040 | Fenced code blocks should have a specified language (bash, text, markdown, etc.) | default                         |
| MD041 | First line in file should be a top level heading.                                | default                         |
| MD042 | No empty links.                                                                  | default                         |
| MD043 | Required heading structure. Not applicable.                                      | default                         |
| MD044 | Proper name capitalization. (handled by `vale` in this case, so ignored.)        | default                         |
| MD045 | All images should have alternate text.                                           | default                         |
| MD046 | Consistent code blocks. Rocky Linux uses fenced code blocks.                     | default                         |
| MD047 | Files should end with a single newline character.                                | default                         |
| MD048 | Use consistent fenced code block style.                                          | default                         |
| MD049 | Use consistent emphasis style. (italic)                                          | default                         |
| MD050 | Use consistent emphasis style. (bold)                                            | default                         |
| MD051 | Links within document. Not applicable. Do not use links within documents.        | default                         |
| MD052 | Pertains to reference links and labels. Not applicable to Rocky Linux.           | default                         |
| MD053 | Needs link and reference definitions.                                            | default                         |
| MD054 | Link and image style. Defined by material-mkdocs within Rocky Linux. Ignored.    | default                         |
| MD055 | Use consistent table style. Forced within material-mkdocs. Ignored.              | default                         |
| MD056 | Table column count must match.                                                   | default                         |
| MD058 | Enforces blank lines around tables.                                              | default                         |

The modification of indentation spaces with rule **MD007** is for proper indenting within admonitions. Rocky Linux documentation does not require line-length rules (**MD013**).

When you trip a markdownlint rule, you will receive an error with the rule number:

![markdownlint_rule_error](../assets/img/interface_with_rule_tripped.png)
