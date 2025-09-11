---
title: Language Server Protocol
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!-- vale off -->
## Introduzione

Il file `lsp.lua` rappresenta una componente fondamentale della configurazione **Rocksmarker** per la gestione e l'integrazione del **Language Server Protocol (LSP)**. Il suo scopo principale è configurare e ottimizzare l'interazione tra Neovim e i vari *language server*, strumenti che forniscono funzionalità avanzate come il completamento automatico del codice, la navigazione tra definizioni, la segnalazione di errori in tempo reale, il refactoring e molto altro.  
Attraverso questo file, vengono definiti i *server* specifici per diversi linguaggi di programmazione (ad esempio Lua, Markdown, YAML, Bash), le loro capacità, e le associazioni di tasti personalizzate per accedere rapidamente alle funzionalità LSP.

Inoltre, il file si occupa di integrare strumenti aggiuntivi come **Mason**, un gestore di pacchetti per l'installazione e la gestione dei *language server* e degli strumenti di linting/formattazione, e **blink.cmp**, un sistema di completamento automatico avanzato. L'obiettivo è creare un ambiente di sviluppo fluido, personalizzabile e altamente produttivo, sfruttando appieno le potenzialità di Neovim e del protocollo LSP.

### Configurazione di `nvim-lspconfig`

Questa sezione definisce le associazioni di tasti (*keymap*) e i comportamenti attivati quando un *language server* si attacca a un buffer in Neovim. Il codice utilizza l'evento `LspAttach` per mappare comandi utili come la navigazione alle definizioni (`gd`), la visualizzazione della documentazione (`K`), il rinominare simboli (`<leader>rn`), e l'accesso a riferimenti, implementazioni e simboli del documento o dello spazio di lavoro tramite **Telescope**.  
Inoltre, se il server supporta l'*highlighting* dei documenti, vengono attivati automaticamente gli *highlight* durante lo spostamento del cursore.

```lua
vim.api.nvim_create_autocmd("LspAttach", {
    group = vim.api.nvim_create_augroup("LspAttachMap", { clear = true }),
    callback = function(event)
        local map = function(keys, func, desc)
            vim.keymap.set("n", keys, func, { buffer = event.buf, desc = "Lsp: " .. desc })
        end
        map("gd", vim.lsp.buf.definition, "Goto Definition")
        map("K", vim.lsp.buf.hover, "Hover Documentation")
        -- ... altre mappe ...
    end,
})
```

### Configurazione delle Capabilities* del LSP

Questa sezione definisce le *capacità* che Neovim comunica ai *language server* per abilitare funzionalità avanzate, come il supporto ai *snippet*, la documentazione in formato Markdown, il completamento automatico con dettagli aggiuntivi, e la gestione delle modifiche al testo.  
Le *capabilities* vengono estese tramite *blink.cmp*, un plugin per il completamento automatico, per garantire che i server LSP forniscano il massimo livello di integrazione.

```lua
local capabilities = require("blink.cmp").get_lsp_capabilities()
capabilities.textDocument.completion.completionItem = {
    documentationFormat = { "markdown", "plaintext" },
    snippetSupport = true,
    preselectSupport = true,
    -- ... altre opzioni ...
}
```

---

### Configurazione dei Language Server Specifici

Questa sezione *configura e avvia* i *language server* per linguaggi specifici:

- **lua_ls**: Server per Lua, con impostazioni per ignorare gli errori relativi alla variabile globale `vim`.
- **vale_ls**: Server per la validazione di file Markdown e messaggi di commit Git.
- **harper_ls**: Server per il controllo grammaticale e stilistico, con opzioni per dizionari personalizzati, linting avanzato (ad esempio controllo ortografico, frasi lunghe, parole ripetute) e severità delle diagnosi impostata su "hint".

```lua
vim.lsp.config("lua_ls", {
    capabilities = capabilities,
    settings = { Lua = { diagnostics = { globals = { "vim" } } } },
})
vim.lsp.config("vale_ls", { capabilities = capabilities, filetypes = { "markdown", "gitcommit" } })
vim.lsp.config("harper_ls", {
    settings = {
        userDictPath = vim.fn.stdpath("config") .. "/spell/exceptions.utf-8.add",
        linters = { SpellCheck = true, LongSentences = true, RepeatedWords = true },
        -- ... altre impostazioni ...
    },
})
```

---

### Configurazione di *mason* e *mason-lspconfig*

Questa sezione utilizza **Mason**, un gestore di pacchetti per Neovim, per installare e gestire automaticamente i *language server* e gli strumenti di sviluppo. Vengono specificati i server da installare (ad esempio `lua_ls`, `html`, `cssls`, `harper_ls`) e viene configurato un *handler* generico per assicurarsi che ogni server venga avviato con le *capabilities* definite in precedenza.

```lua
require("mason").setup({})
require("mason-lspconfig").setup({
    ensure_installed = { "lua_ls", "html", "cssls", "marksman", "harper_ls", "yamlls", "bashls", "taplo" },
    handlers = {
        function(server_name)
            vim.lsp.config("[lsp]", { capabilities = capabilities })
        end,
    },
})
```

---

### Configurazione di *mason-tool-installer*

Questa sezione estende Mason per installare *strumenti aggiuntivi* utili per il linting e la formattazione del codice, come `markdownlint`, `vale`, `stylua`, `prettier`, e `shellcheck`. Questi strumenti vengono installati automaticamente all'avvio di Neovim, garantendo che l'ambiente di sviluppo sia sempre pronto all'uso.

```lua
require("mason-tool-installer").setup({
    ensure_installed = {
        "markdownlint", "vale", "stylua", "shfmt", "yamlfmt",
        "shellcheck", "prettier", "yamllint"
    },
})
```

### Configurazione di *blink.cmp* per il completamento automatico

Questa sezione configura **blink.cmp**, un plugin per il completamento automatico avanzato. Vengono definite le *associazioni di tasti* (ad esempio `super-tab` per la navigazione tra le opzioni), l'aspetto del menu di completamento, e le *fonti* da cui attingere le suggerimenti (LSP, buffer, snippet, path). Il plugin è configurato per mostrare automaticamente il menu di completamento e per integrare il *ghost text* (testo grigio che anticipa il completamento).

```lua
require("blink.cmp").setup({
    keymap = { preset = "super-tab", ["<ESC>"] = { "cancel", "fallback" } },
    completion = {
        menu = {
            scrollbar = false,
            ghost_text = { enabled = true },
            -- ... altre opzioni ...
        },
    },
    sources = { default = { "lsp", "path", "snippets", "buffer" } },
})
```

## Supporto LSP in Rocksmarker

In sintesi, il file **`lsp.lua`** rappresenta il cuore della configurazione LSP all'interno dell'ambiente Neovim personalizzato **Rocksmarker**, offrendo una soluzione integrata e altamente personalizzabile per lo sviluppo software e la scrittura di documentazione. Attraverso l'utilizzo di plugin come **`nvim-lspconfig`**, **`mason`**, **`mason-lspconfig`**, **`mason-tool-installer`**, e **`blink.cmp`**, questo file non solo abilita il supporto avanzato per numerosi linguaggi di programmazione e formati di file, ma ottimizza anche l'esperienza utente con keymap intuitive, completamento automatico intelligente e strumenti di analisi statica. L'obiettivo finale è creare un ambiente di sviluppo **coeso, efficiente e adattabile**, che consenta agli utenti di concentrarsi sulla scrittura del codice e sulla produttività, sfruttando al massimo le potenzialità di Neovim e dell'ecosistema LSP.
