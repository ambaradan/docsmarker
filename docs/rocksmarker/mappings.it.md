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
Con "*silent*" impostato su **true**, lo script nasconde l'eco dei comandi, fornendo un'interfaccia utente più pulita. La mappa dei tasti è impostata per attivare l'operazione di salvataggio quando l'utente preme la chiave *leader* seguita da "**s**" in modalità normale, con un commento descrittivo opzionale che spiega il suo scopo. Questo approccio migliora il flusso di lavoro dell'utente fornendo una metodologia semplificata per il salvataggio dei file con un minimo di tasti premuti e un'efficienza migliorata nell'ambiente di editing di testo di Neovim.

Nell'esempio precedente è stato utilizzato un comando ma le mappature accettano anche l'uso delle funzioni per una maggiore flessibilità e la stessa funzionalità del comando precedente può essere scritta nel modo seguente:

```lua
-- Key mapping to perform a save function
vim.keymap.set('n', '<leader>s', function()
    -- Save function
    vim.cmd('w')
    vim.notify("File saved successfully!", vim.log.levels.INFO)
end, { noremap = true, silent = true })
```

Quando viene premuta la combinazione di tasti **Spazio + s** viene eseguita la funzione associata, all'interno di questa funzione il comando **vim.cmd('w')** salva il file corrente senza uscire dall'editor, consentendo all'utente di continuare a lavorare sul file dopo averlo salvato. Successivamente, la funzione **vim.notify** visualizza una notifica all'utente, informandolo che il file è stato salvato con successo. La notifica è classificata come di tipo "info", che normalmente la mostra con un colore o uno stile che indica un'informazione utile ma non critica.  
Le opzioni { noremap = true, silent = true } associate a questa mappatura di tasti servono a migliorare la sua funzionalità e a prevenire potenziali problemi.

## Mappatura in Rocksmaker

Per semplificare ulteriormente la scrittura delle mappature in Rocksmarker sono fornite alcune funzioni dedicate presenti nel file `lua/utils/editor.lua`. Lo scopo del codice è quello di definire funzioni utili per la gestione delle mappature dei tasti all'interno dell'ambiente Neovim. La funzione **M.set_key_mapping** in particolare costituisce l'interfaccia principale per l'impostazione di nuove mappature di tasti.

Le funzioni definite inoltre consentono di superare la limitazione di quattro componenti che normalmente limita l'uso di vim.keymap.set, fornendo un modo più flessibile e organizzato per configurare le mappature dei tasti in Neovim.

### Funzioni di mappatura

Il codice può essere consultato nel file `lua/utils/editor.lua`, questo file è utilizzato per raccogliere tutte le funzioni personalizzate dedicate alla gestione di Rocksmarker. Le funzioni sono importate con la funzione Lua *require*:

```lua
local editor = require("utils.editor")
```

E sono le seguenti:

```lua
--- Function to set key mappings with options
function M.set_key_mapping(mode, lhs, rhs, desc)
 local opts = M.make_opt(desc)
 M.remap(mode, lhs, rhs, opts)
end

--- Local function to remap keybinding.
M.remap = function(mode, lhs, rhs, opts)
 pcall(vim.keymap.del, mode, lhs)
 return vim.keymap.set(mode, lhs, rhs, opts)
end

--- Function to create default options for key mappings.
function M.make_opt(desc)
 return {
  silent = true,
  noremap = true,
  desc = desc,
 }
end
```

`M.set_key_mapping(mode, lhs, rhs, desc)`

:    Questa funzione semplifica la creazione della mappatura dei tasti, riceve il **mode** (modalità - ad esempio, normale, inserimento, visualizzazione), **lhs** (la combinazione di tasti che si desidera mappare), **rhs** (il comando o l'azione da eseguire) e **desc** (una descrizione per la mappatura). Converte infine la descrizione (*desc)* in un'opzione usando *M.make_opt(desc)* e poi invoca la successiva funzione *M.remap(mode, lhs, rhs, opts)*.

`M.remap(mode, lhs, rhs, opts)`

:    Questa funzione gestisce effettivamente la mappatura dei tasti. Inizialmente, utilizza *pcall* per tentare di rimuovere eventuali mappature esistenti usando *vim.keymap.del*. Questo è importante per evitare conflitti con mappature precedenti. Quindi, imposta la nuova mappatura usando **vim.keymap.set(mode, lhs, rhs, opts)**. Qui, *opts* è l'oggetto delle opzioni creato in *M.make_opt(desc)*.

`M.make_opt(desc)`

:    Crea e restituisce un set di opzioni di base per le mappature dei tasti. Le opzioni specificate includono:

- **silent**: per prevenire l'eco dell'azione nella linea di comando.
- **noremap**: per assicurarsi che la mappatura non venga ulteriormente interpretata, mantenendo la sua logica.
- **desc**: per fornire una descrizione, utile per ottenere aiuto o per la documentazione.

!!! Note "Superamento della limitazione dei quattro Componenti"

    Le mappature dei tasti in Neovim sono normalmente limitate nei loro parametri a sole quattro componenti: modalità, la stringa di tasti a sinistra, la stringa di tasti a destra e le opzioni. Le funzioni di cui sopra aiutano a superare questa limitazione in quanto:

    - **Separano la logica**: La logica di creazione della mappatura è separata dalla logica di definizione delle opzioni, permettendo di mantenere un codice più pulito e gestibile.  
    - **Consentono la loro riutilizzabilità**: Le funzioni possono essere riutilizzate per differenti mappature senza il bisogno di ripetere il codice per gestire le opzioni, riducendo ridondanza e migliorando la manutenibilità.  
    - **Facilitano la loro estensione**: È possibile facilmente aggiungere ulteriori opzioni o modificare le funzionalità senza dover modificare la parte fondamentale di vim.keymap.set, mantenendo così la compatibilità con le future versioni di 
    Neovim.

### Scrittura delle mappature

Con le funzioni sopra descritte il comando iniziale:

```lua
vim.keymap.set('n', '<leader>s', ':w<CR>', {
    noremap = true,
    silent = true,
    desc = "Quick save mapping"
})
```

Diventa:

```lua
editor.remap('n', '<leader>s', ':w<CR>', editor.make_opt("Quick save mapping"))
```
