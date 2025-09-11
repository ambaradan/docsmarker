---
title: Common Utilities
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

<!-- vale off -->
## Introduzione

Il file `utils.lua`, rappresenta un modulo fondamentale all'interno della configurazione **Rocksmarker**. Il suo scopo principale è centralizzare e ottimizzare la configurazione di una serie di *plugin di utilità* che arricchiscono l'esperienza di editing, migliorando sia la produttività che l'efficienza nell'utilizzo dell'editor.

Il file si occupa di definire e personalizzare le impostazioni per plugin essenziali come *telescope* (per la ricerca e la navigazione avanzata), *persisted* (per la gestione delle sessioni), *toggleterm* (per l'integrazione di terminali interattivi), *neo-tree.nvim* (per la navigazione dei file in stile tree-view), *neogit* (per la gestione di Git), *spectre* (per la ricerca e sostituzione globale), *yanky* (per la gestione avanzata degli appunti), *indent-blankline* e *rainbow-delimiters* (per una migliore visualizzazione del codice), e altri ancora.

In sintesi, `utils.lua` funge da **punto di controllo centralizzato** per una configurazione avanzata e ottimizzata di Neovim, rendendo l'editing del codice più fluido, intuitivo e potente.

### `telescope.nvim`

Il plugin **telescope.nvim** è uno strumento di ricerca e navigazione avanzato per Neovim, che consente di trovare file, buffer, commit Git, e molto altro in modo rapido ed efficiente. Nel file `utils.lua`, la configurazione di Telescope include la personalizzazione delle *azioni predefinite* (come la chiusura con `<esc>`), la definizione di *layout* (ad esempio, l'altezza della finestra orizzontale impostata a `0.85` dello schermo), e la configurazione di *temi* specifici per diversi picker (come `dropdown` per i buffer e `ivy` per lo stato Git). Vengono inoltre abilitate diverse *estensioni*, tra cui `file_browser`, `frecency`, `ui-select`, e `undo`, ognuna con impostazioni personalizzate per migliorare l'usabilità e l'aspetto visivo.

```lua
local actions = require("telescope.actions")
require("telescope").setup({
    defaults = {
        mappings = {
            i = { ["<esc>"] = actions.close }
        },
        layout_config = {
            horizontal = { height = 0.85 }
        }
    },
    pickers = {
        buffers = { sort_lastused = true, theme = "dropdown" },
        git_status = { theme = "ivy" }
    },
    extensions = {
        file_browser = { theme = "ivy", hijack_netrw = true },
        frecency = { show_scores = true, theme = "dropdown" }
    }
})
```

### `persisted.nvim`

**Persisted.nvim** è un plugin che consente di *salvare e ripristinare le sessioni di lavoro* in Neovim, in modo da poter riaprire i file e i layout esattamente come erano stati lasciati. Nella configurazione, è stato impostato `autoload = false` per evitare il caricamento automatico delle sessioni all'avvio, garantendo così un controllo manuale su quando ripristinare una sessione. Inoltre, è stata abilitata l'estensione per *telescope*, che permette di cercare e selezionare le sessioni salvate direttamente dall'interfaccia di Telescope.

```lua
require("persisted").setup({ autoload = false })
require("telescope").load_extension("persisted")
```

### `toggleterm.nvim`

**Toggleterm.nvim** offre un modo flessibile per integrare *terminali interattivi* all'interno di Neovim, consentendo di aprire e chiudere terminali con facilità.  
Nella configurazione, il terminale è impostato per aprirsi in modalità *float* con dimensioni proporzionali allo schermo (`width = 50%`, `height = 40%`) e un bordo arrotondato (`border = "curved"`). È stato definito un *mapping globale* (`<c-t>`) per aprire rapidamente il terminale, e sono state configurate opzioni di trasparenza (`winblend = 10`) e posizionamento automatico. Il terminale si chiude automaticamente all'uscita (`close_on_exit = true`) e si avvia in modalità inserimento (`start_in_insert = true`).

```lua
require("toggleterm").setup({
    size = function(term)
        if term.direction == "horizontal" then
            return vim.o.lines * 0.4
        else
            return vim.o.columns * 0.4
        end
    end,
    open_mapping = [[<c-t>]],
    direction = "float",
    float_opts = {
        border = "curved",
        winblend = 10
    }
})
```

### `neo-tree.nvim`

**Neo-tree.nvim** è un plugin che fornisce una *visualizzazione ad albero* dei file e delle directory, simile a NERDTree o Explorer, ma con un'interfaccia più moderna e personalizzabile. Nella configurazione, Neo-tree è impostato per aprirsi sul *lato destro* dello schermo (`position = "right"`) con una larghezza di `50` colonne. È stato inoltre definito uno stile per il bordo delle finestre *popup* (`popup_border_style = "rounded"`), che migliora l'aspetto visivo dell'interfaccia.

```lua
require("neo-tree").setup({
    popup_border_style = "rounded",
    window = {
        position = "right",
        width = 50
    }
})
```

### `neogit`

**Neogit** è un'interfaccia avanzata per **Git** all'interno di Neovim, che consente di gestire repository, visualizzare lo stato dei file, e effettuare operazioni Git in modo interattivo. Nella configurazione, Neogit è impostato per aprirsi in una **scheda** (`kind = "tab"`) e integra funzionalità aggiuntive come **Telescope** e **Diffview**. È stato inoltre configurato per mostrare fino a `20 commit recenti` nello stato del repository e per utilizzare uno stile di grafico Unicode (`graph_style = "unicode"`).

```lua
require("neogit").setup({
    kind = "tab",
    graph_style = "unicode",
    integrations = { telescope = true, diffview = true },
    status = { recent_commit_count = 20 }
})
```

### `spectre.nvim`

**Spectre** è un plugin che facilita la *ricerca e sostituzione globale* di testo all'interno di un progetto. Nella configurazione, è stata disabilitata l'opzione `live_update`, che impedisce l'esecuzione automatica della ricerca ogni volta che un file viene modificato. Questo consente di avere un controllo più preciso sul momento in cui eseguire la ricerca e la sostituzione.

```lua
require("spectre").setup({ live_update = false })
```

### `yanky.nvim`

**Yanky** è un plugin che estende le funzionalità degli *appunti* in Neovim, consentendo di gestire una cronologia delle operazioni di copia (`yank`) e incolla (`put`). Nella configurazione, è stato impostato un **limite di 200 voci** nella cronologia (`history_length = 200`) e abilitata la sincronizzazione con i registri numerati di Vim (`sync_with_numbered_registers = true`). È stata inoltre abilitata l'evidenziazione visiva durante le operazioni di copia e incolla (`highlight = { on_put = true, on_yank = true }`).

```lua
require("yanky").setup({
    highlight = { on_put = true, on_yank = true },
    ring = {
        history_length = 200,
        sync_with_numbered_registers = true
    }
})
```

### `indent-blankline.nvim`

**Indent-blankline** è un plugin che aggiunge *linee verticali* per visualizzare chiaramente i livelli di indentazione nel codice, migliorando la leggibilità.  
Nella configurazione, è stato definito un carattere personalizzato (`char = "│"`) per le linee di indentazione e sono stati esclusi alcuni tipi di file e buffer (come `help`, `terminal`, e `TelescopePrompt`) per evitare interferenze con altre visualizzazioni.

```lua
require("ibl").setup({
    indent = { highlight = "IblIndent", char = "│" },
    exclude = {
        filetypes = { "help", "terminal", "TelescopePrompt" }
    }
})
```

### `rainbow-delimiters.nvim`

**Rainbow-delimiters** è un plugin che *colora le parentesi e i delimitatori* nel codice con colori diversi, in modo da renderne più semplice l'identificazione e l'abbinamento. Nella configurazione, è stata definita una **strategia globale** per la maggior parte dei linguaggi e una strategia locale per Lua. Sono stati inoltre specificati i nomi degli highlight group per i colori da utilizzare (ad esempio, `RainbowDelimiterRed`, `RainbowDelimiterYellow`).

```lua
require("rainbow-delimiters.setup").setup({
    strategy = {
        [""] = require("rainbow-delimiters").strategy["global"],
        vim = require("rainbow-delimiters").strategy["local"]
    },
    highlight = {
        "RainbowDelimiterRed",
        "RainbowDelimiterYellow",
        "RainbowDelimiterBlue"
    }
})
```

### `nvim-autopairs`

**Nvim-autopairs** è un plugin che *autocompleta automaticamente* le parentesi, le virgolette e altri delimitatori durante la digitazione, migliorando la velocità di scrittura del codice. Nella configurazione, è stato disabilitato per alcuni tipi di file, come `TelescopePrompt` e `vim`, per evitare comportamenti indesiderati in contesti specifici.

```lua
require("nvim-autopairs").setup({
    disable_filetype = { "TelescopePrompt", "vim" }
})
```

### `nvim-highlight-colors`

**Nvim-highlight-colors** è un plugin che *evidenzia i colori* presenti nel codice (ad esempio, i valori esadecimali o i nomi dei colori) con un'anteprima visiva del colore stesso. Nella configurazione, è stato impostato il metodo di rendering su `virtual`, che visualizza il colore come un'evidenziazione virtuale a fianco del testo.

```lua
require("nvim-highlight-colors").setup({
    render = "virtual"
})
```

### Editing ottimizzato

In sintesi, il file `utils.lua` offre una soluzione integrata e altamente personalizzabile per ottimizzare l'esperienza di editing. Attraverso la configurazione accurata di plugin essenziali come **Telescope**, **Persisted**, **Toggleterm**, **Neo-tree**, **Neogit**, **Spectre**, **Yanky**, **Indent-blankline**, **Rainbow-delimiters**, **Nvim-autopairs** e **Nvim-highlight-colors**, questo file trasforma Neovim in un ambiente di sviluppo *potente, flessibile e user-friendly*. Ogni plugin è stato configurato con attenzione ai dettagli, bilanciando funzionalità avanzate, usabilità e coerenza visiva, per garantire un flusso di lavoro *produttivo e privo di distrazioni*.

Grazie a questa configurazione, gli utenti possono beneficiare di una *navigazione efficiente* tra file e buffer, una *gestione avanzata delle sessioni e dei terminali*, un *controllo di versione integrato*, una *visualizzazione chiara del codice* e strumenti per *ricerca, sostituzione e gestione degli appunti*. In definitiva, `utils.lua` non solo semplifica la configurazione di Neovim, ma eleva l'editing del codice a un livello superiore, rendendolo più **intuitivo, personalizzato e potente**.
