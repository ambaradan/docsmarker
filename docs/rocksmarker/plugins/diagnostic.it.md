---
title: Qualità del codice
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!-- vale off-->
## Introduzione

Il file `diagnostics.lua` serve a centralizzare e ottimizzare la gestione della *qualità del codice* durante lo sviluppo, integrando tre strumenti fondamentali:

- **conform.nvim** che si occupa della *formattazione automatica* del codice, garantendo che i file rispettino stili coerenti e predefiniti per diversi linguaggi (come Lua, CSS, HTML, Bash e Markdown).
- **nvim-lint** che aggiunge un livello di *linting*, ovvero l’analisi statica del codice per rilevare errori di sintassi, potenziali bug o violazioni di standard specifici (ad esempio, tramite *markdownlint* per Markdown o *shellcheck* per script Bash).
- **trouble.nvim** che offre una visualizzazione chiara e interattiva delle *diagnostiche* (errori, avvisi, suggerimenti) generate da LSP (*Language Server Protocol*) o dai linter, migliorando l’efficienza nella correzione e nel debugging.

In sintesi, questo file trasforma Neovim in un ambiente di sviluppo *proattivo*, dove la formattazione, il linting e la visualizzazione delle diagnostiche avvengono in modo automatico e integrato, riducendo il carico cognitivo sullo sviluppatore e promuovendo codice pulito, funzionale e conforme agli standard.

### conform.nvim: Formattazione Automatica del Codice

Il plugin **conform.nvim** è configurato per applicare la formattazione automatica ai file in base al loro tipo (*filetype*). Nel blocco di codice seguente, vengono definiti i *formatter* specifici per ogni linguaggio supportato:

```lua
require("conform").setup({
 formatters_by_ft = {
  lua = { "stylua" },
  css = { "prettier" },
  html = { "prettier" },
  sh = { "shfmt" },
  bash = { "shfmt" },
  markdown = { "markdownlint" },
  yaml = { "yamlfmt" },
 },
 format_on_save = {
  timeout_ms = 1000,
  lsp_format = "fallback",
 },
})
```

Qui, ad esempio, i file Lua vengono formattati con *stylua*, mentre CSS e HTML utilizzano *prettier*. La direttiva `format_on_save` abilita la formattazione automatica al salvataggio del file, con un timeout di 1 secondo e la possibilità di ricorrere al formatter integrato nel *Language Server Protocol* (LSP) come soluzione di backup.

### nvim-lint: Analisi Statica e Linting

**nvim-lint** si occupa di eseguire controlli statici sul codice per identificare errori, avvisi o violazioni di standard. La configurazione seguente associa i *linter* ai rispettivi linguaggi:

```lua
require("lint").linters_by_ft = {
 markdown = { "markdownlint", "vale" },
 yaml = { "yamllint" },
 bash = { "shellcheck" },
}
vim.api.nvim_create_autocmd({ "BufWritePost" }, {
 callback = function()
  require("lint").try_lint()
 end,
})
```

Per esempio, i file Markdown vengono analizzati con *markdownlint* e *vale*, mentre gli script Bash sono verificati con *shellcheck*. L’evento `BufWritePost` triggera automaticamente il linting ogni volta che un file viene salvato, garantendo un feedback immediato sul codice scritto.

### trouble.nvim: Visualizzazione delle Diagnostiche

**trouble.nvim** è un plugin che migliora la visualizzazione delle diagnostiche (errori, avvisi, suggerimenti) generate da LSP o dai linter. La sua configurazione è semplice ma efficace:

```lua
require("trouble").setup()
```

Questa riga inizializza il plugin con le impostazioni predefinite, offrendo una lista interattiva e navigabile delle diagnostiche direttamente nell’editor. Trouble.nvim permette di filtrare, ordinare e accedere rapidamente ai problemi rilevati nel codice, ottimizzando il processo di debugging e correzione. La sua integrazione con gli altri plugin (Conform e nvim-lint) crea un flusso di lavoro coerente e produttivo.

## Un ecosistema integrato per lo sviluppo di qualità

In sintesi, la configurazione di `diagnostics.lua` trasforma Neovim in un ambiente di sviluppo *potente e automatizzato*, dove *conform.nvim*, *nvim-lint* e *trouble.nvim* lavorano in sinergia per garantire codice sempre **formattato, analizzato e privo di errori**.  
Questa combinazione non solo riduce il tempo dedicato a operazioni manuali e ripetitive, ma eleva anche la qualità del codice, rendendo il processo di sviluppo più fluido, efficiente e affidabile. Che si stia lavorando su script Bash, documentazione Markdown o codice Lua, questo setup assicura che ogni file sia coerente, conforme agli standard e pronto per la produzione.
