---
title: Markdown writing assistant
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

<!-- vale off -->
## Introduzione

Il file `markdown.lua` ha lo scopo principale di ottimizzare e potenziare l'esperienza di editing dei file **Markdown** all'interno dell'ambiente Neovim, sfruttando una serie di plugin dedicati.

Il file configura quattro plugin:

- **render-markdown.nvim**: per migliorare la visualizzazione degli elementi Markdown, come intestazioni, blocchi di codice e tabelle, rendendoli più leggibili e strutturati.
- **markdown.nvim**: per fornire funzionalità avanzate per la manipolazione del testo in modalità Markdown, come scorciatoie da tastiera per formattare rapidamente il testo (grassetto, corsivo, codice inline, ecc.).
- **markdown-table-mode.nvim**: per facilitare la creazione e la modifica di tabelle in formato Markdown, automatizzando l'allineamento e la formattazione.
- **zen-mode.nvim**: per offrire una modalità di scrittura "zen", riducendo le distrazioni visive e concentrando l'attenzione sul contenuto.

Questa configurazione è pensata per utenti che desiderano un flusso di lavoro efficiente e personalizzato per la scrittura e la gestione di documenti Markdown in Neovim, combinando produttività e chiarezza visiva.

### `render-markdown.nvim`

Il plugin **render-markdown.nvim** è configurato per migliorare la resa visiva degli elementi Markdown all'interno di Neovim. Nel codice, viene definita la formattazione delle **intestazioni** (*heading*), che includono icone personalizzate per i diversi livelli (ad esempio, `⌂` per il livello 1, `¶` per il livello 2, ecc.), bordi visivi per evidenziare i blocchi, e spaziature laterali per una migliore leggibilità. I blocchi di codice (`code`) sono configurati con un'ampiezza minima di 45 caratteri e spaziature laterali, mentre le tabelle (`pipe_table`) vengono visualizzate in stile "*normale*". La funzionalità LaTeX è disabilitata (`latex = { enabled = false }`), in quanto non necessaria per questo contesto.

```lua
require("render-markdown").setup({
    heading = {
        sign = false,
        icons = { "⌂ ", "¶ ", "§ ", "❡ ", "⁋ ", "※ " },
        width = "block",
        border = true,
        border_virtual = true,
        left_pad = 2,
        right_pad = 4,
    },
    code = { sign = false, width = "block", left_pad = 2, right_pad = 4, min_width = 45 },
    pipe_table = { style = "normal" },
    latex = { enabled = false },
})
```

### `markdown.nvim`

Il plugin **markdown.nvim** introduce funzionalità avanzate per la manipolazione del testo in formato Markdown. Nella configurazione, viene definita una funzione `toggle` che consente di attivare o disattivare rapidamente la formattazione del testo (grassetto, corsivo, codice inline, ecc.) tramite scorciatoie da tastiera. Ad esempio, la combinazione ++ctrl+"b"++ applica il **grassetto**, ++ctrl+"i"++ il *corsivo*, ++ctrl+grave-accent++ il `codice inline`, e ++ctrl+"s"++ ~~il barrato~~. Queste scorciatoie sono attive solo nella modalità **Visual** e sono specifiche per il buffer corrente, garantendo un editing rapido e intuitivo.

```lua
require("markdown").setup({
    on_attach = function(bufnr)
        local function toggle(key)
            return "<Esc>gv<Cmd>lua require'markdown.inline'.toggle_emphasis_visual'" .. key .. "'<CR>"
        end
        vim.keymap.set("x", "<C-b>", toggle("b"), { buffer = bufnr })
        vim.keymap.set("x", "<C-i>", toggle("i"), { buffer = bufnr })
        vim.keymap.set("x", "<C-`>", toggle("c"), { buffer = bufnr })
        vim.keymap.set("x", "<C-s>", toggle("s"), { buffer = bufnr })
    end,
})
```

### `markdown-table-mode.nvim`

Il plugin **markdown-table-mode.nvim** è dedicato alla gestione delle tabelle in formato Markdown. La configurazione specifica che il plugin deve essere attivo per tutti i file con estensione `.md` (`filetype = { "*.md" }`). Le opzioni `insert = true` e `insert_leave = true` consentono di attivare automaticamente la modalità tabella quando si digita il carattere `|` (utilizzato per definire le colonne) e di mantenere la formattazione anche quando si esce dalla modalità di inserimento. Questo semplifica la creazione e la modifica delle tabelle, garantendo un allineamento automatico e una struttura coerente.

```lua
require("markdown-table-mode").setup({
    filetype = { "*.md" },
    options = {
        insert = true,
        insert_leave = true,
    },
})
```

### `zen-mode.nvim`

Il plugin **zen-mode.nvim** offre una modalità di scrittura "*zen*", progettata per ridurre al minimo le distrazioni e massimizzare la concentrazione sul contenuto.  
Nella configurazione, la finestra di editing viene impostata a una larghezza dell'85% dello schermo (`width = 0.85`), mentre le opzioni di visualizzazione disattivano i numeri di riga (`number = false`) e la numerazione relativa (`relativenumber = false`), mantenendo attiva solo la visualizzazione dei caratteri speciali (`list = true`).  
Inoltre, la barra di stato viene nascosta (`laststatus = 0`) per eliminare ulteriori distrazioni visive, creando un ambiente di scrittura pulito e focalizzato.

```lua
require("zen-mode").setup({
    window = {
        width = 0.85,
        options = {
            number = false,
            list = true,
            relativenumber = false,
        },
    },
    plugins = {
        options = {
            laststatus = 0,
        },
    },
})
```

## Markdown-specific environment

In sintesi, la configurazione di `markdown.lua` rappresenta un esempio di come Neovim possa essere personalizzato per offrire un ambiente di editing Markdown altamente efficiente e confortevole.  
Grazie all'integrazione di *render-markdown.nvim*, *markdown.nvim*, *markdown-table-mode.nvim* e *zen-mode.nvim*, l'utente beneficia di una visualizzazione chiara e strutturata dei documenti, di scorciatoie rapide per la formattazione, di strumenti avanzati per la gestione delle tabelle e di una modalità di scrittura priva di distrazioni.  
Questa combinazione di plugin non solo semplifica il flusso di lavoro, ma eleva anche la qualità e la produttività nella creazione di contenuti in formato Markdown, rendendo Neovim uno strumento versatile e potente per scrittori, sviluppatori e professionisti che lavorano con documentazione tecnica o testuale.
