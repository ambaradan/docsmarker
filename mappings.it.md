---
title: Mappings
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!--vale off-->

## Introduzione

Le mappature dei tasti di Neovim sono un meccanismo per personalizzare le interazioni con l'editor, consentono di definire scorciatoie da tastiera uniche per le varie modalità operative. Il sistema di mappatura di Neovim è strutturato intorno a diverse configurazioni specifiche di modalità, ognuna delle quali serve a scopi distinti nella manipolazione del testo e nella navigazione dell'editor

### Tipi di Mappature

Ogni tipo di mappatura sfrutta la funzione *vim.keymap.set()*, che accetta opzioni di modalità, combinazione di tasti, azione e configurazione, fornendo un sistema di mappatura flessibile ed estensibile. Di seguito sono elencate le modalità e il loro utilizzo comune.

Le mappature in modalità **NORMAL** (**n**) sono utilizzate principalmente per le interazioni basate sui comandi, consentono una navigazione rapida, la gestione dei file e le operazioni a livello di sistema senza entrare in modalità INSERT.  
Le mappature in modalità **INSERT** (**i**) sono normalmente utilizzate per l'immissione di testo, consentono la creazione di scorciatoie complesse per la digitazione o per il  completamento automatico.  
Le mappature in modalità **VISUAL** (**v**) facilitano la selezione e la trasformazione del testo, questo consente l'attivazione delle funzionalità di modifica durante le interazioni basate sull'evidenziazione.  
Le mappature in modalità **TERMINAL** (**t**) offrono funzionalità per la gestione dei terminali integrati, consentono transizioni ed esecuzioni di comandi personalizzati all'interno del terminale di Neovim.  
Le mappature in modalità **COMMAND** (**c**) forniscono le interazioni con la riga di comando, supportano operazioni di ricerca e sostituzione.  
Le mappature in modalità **VISUAL BLOCK** (**x**) forniscono un controllo granulare sulle selezioni di testo rettangolare e sulle trasformazioni a livello di blocco.

### Gerarchia delle mappature

La gerarchia delle mappature di Neovim fornisce un sistema a più livelli per organizzare e implementare le interazioni da tastiera, fornendo un controllo granulare sul comportamento dell'editor in diversi contesti e modalità. Le mappature globali fungono da livello primario di configurazione, stabilendo scorciatoie da tastiera universali che si applicano in modo coerente a tutti i buffer e a tutti i tipi di file. Queste mappature globali creano un modello di interazione di base che può essere successivamente perfezionato con strategie di mappatura più specialistiche.  
Le mappature buffer-local introducono un approccio più sfumato, consentendo scorciatoie da tastiera specifiche per il contesto, attive solo all'interno di specifici tipi di file o di singoli buffer, permettendo personalizzazioni specifiche per la lingua o per il progetto.

### Sintassi

La sintassi base per la costruzione di una mappatura è la seguente:

```lua
vim.keymap.set('n', '<leader>s', ':w<CR>', {
    noremap = true,   -- Prevent recursive mapping
    silent = true,    -- Suppress command echo
    desc = "Quick save mapping"  -- Optional description
})
```

Il codice Lua definito crea una mappa di tasti personalizzata per Neovim, nello specifico creando una funzionalità di salvataggio veloce utilizzando la chiave *leader*. La mappa dei tasti è configurata con l'opzione "*noremap*" per evitare la mappatura ricorsiva, garantendo un comportamento diretto e prevedibile.  
Con "*silent*" impostato su **true**, lo script nasconde l'eco dei comandi, fornendo un'interfaccia utente più pulita. La mappa dei tasti è impostata per attivare l'operazione di salvataggio quando l'utente preme la chiave *leader* seguita da "**s**" in modalità normale, con un commento descrittivo opzionale che spiega il suo scopo. Questo approccio migliora la workflow dell'utente fornendo una metodologia semplificata per il salvataggio dei file con un minimo di tasti premuti e un'efficienza migliorata nell'ambiente di editing di testo di Neovim.

Nell'esempio precedente è stato utilizzato un comando

```lua
-- Key mapping to perform a save function
vim.keymap.set('n', '<leader>s', function()
    -- Save function
    vim.cmd('w')
    vim.notify("File saved successfully!", "info")
end, { noremap = true, silent = true })
```

In questo esempio, la mappatura di tasti <leader>s esegue una funzione che salva il file attuale con :wq e stampa un messaggio di conferma. La funzione è definita all'interno della mappatura di tasti e viene eseguita quando l'utente preme la chiave leader seguita da 's' in modalità normale. La mappatura di tasti è configurata con noremap per evitare la mappatura ricorsiva e con silent per sopprimere l'eco dei comandi.
