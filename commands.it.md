---
title: Autocommands
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!--vale off-->

## Introduzione

Cosa sono gli autocomandi di Neovim?

Gli autocomandi di Neovim sono una potente funzione che consente di eseguire automaticamente comandi o script in risposta a eventi specifici. Consentono di personalizzare il comportamento dell'editor, di automatizzare le attività ripetitive e di impostare configurazioni specifiche per il tipo di file.

Scopo degli Autocomandi:

- Personalizzare il comportamento dell'editor:  
Gli autocms consentono di automatizzare attività come la formattazione, l'evidenziazione o l'inserimento di testo, in base a eventi specifici.
- Automatizzare le attività ripetitive:  
Utilizzate gli autocms per eseguire attività che normalmente fareste manualmente, risparmiando tempo e fatica.
- Configurazioni specifiche per ogni tipo di file:  
I comandi automatici consentono di definire impostazioni specifiche per diversi tipi di file, come l'indentazione, l'evidenziazione della sintassi o la piegatura.
- Migliorare l'efficienza del flusso di lavoro:
Gli autocmd aiutano a snellire il flusso di lavoro automatizzando le attività e riducendo il lavoro manuale.

### Sintassi

Il codice per la scrittura degli autocomandi, essendo codice Lua, deve rispettare la usa sintassi. Per la creazione degli autocomandi è consigliato l'uso della funzione *vim.api.nvim_create_autocmd()*, inoltre per garantire script efficienti e affidabili, è necessario mantenere leggere le *callback*, utilizzare la gestione degli errori, modularizzare la logica complessa e sfruttare le funzioni API di Neovim.

La sintassi base è la seguente:

```lua
vim.api.nvim_create_autocmd({event}, {
 pattern = {pattern},
 callback = function()
 -- Your code here
end
})
```

I componenti sui quali si può intervenire per creare la funzione sono:

- **event**: È l'evento che attiva l'autocmd. Alcuni eventi comuni sono:
    - *BufEnter*: Quando si entra in un buffer
    - *BufLeave*: Quando si lascia un buffer
    - *FileType*: Quando viene rilevato un tipo di file
    - *TextYankPost*: Dopo aver strattonato (copiato) il testo
    - *TermOpen*: Quando si apre un terminale
- **pattern**: I *pattern* forniscono una corrispondenza flessibile tra file ed eventi, supportano strategie di corrispondenza multiple, possono essere combinati con gruppi e callback e sono essenziali per creare autocomandi sensibili al contesto.
- **callback**: I callback sono funzioni Lua che vengono eseguite quando si verificano eventi specifici. Ciò fornisce un modo programmatico e flessibile di gestire gli eventi.  
Quando una funzione di *callback* viene attivata, riceve una tabella **args** che contiene informazioni specifiche sull'evento. Questa tabella può includere dettagli come il percorso completo del file (*file*), il numero del buffer (*buf*), il pattern abbinato (*match*) e l'ID dell'autocmd (*id*).

!!! Tip "Suggerimento per le prestazioni"

    Creando un gruppo di autocomandi, è possibile evitare la duplicazione degli stessi e migliorare l'efficienza complessiva degli script.

### Augroups

I gruppi di autocomandi in Neovim Lua forniscono un potente meccanismo per organizzare e gestire le configurazioni guidate dagli eventi. Creano un approccio strutturato alla gestione degli eventi dell'editor, consentendo agli sviluppatori di definire con precisione e chiarezza comportamenti complessi e consapevoli del contesto.  
Questo approccio trasforma la gestione degli eventi da un semplice sistema di attivazione-risposta in una strategia di configurazione completa che si adatta a diversi contesti e requisiti di editing.

```lua
-- Core Augroup Creation Mechanism
local my_augroup = vim.api.nvim_create_augroup("CustomEditorConfig", { clear = true })
-- Autocmd Registration Within Group
vim.api.nvim_create_autocmd("BufRead", {
    group = my_augroup,
    pattern = "*.lua",
    callback = function(args)
        -- Contextual event handling
        vim.diagnostic.disable(args.buf)
        vim.opt_local.spell = true
    end
})
-- Add another autocmd to the same augroup
vim.api.nvim_create_autocmd("FileType", {
    group = my_augroup,
    pattern = "python",
    callback = function()
        vim.bo.tabstop = 4
        vim.bo.shiftwidth = 4
        vim.bo.expandtab = true
    end
})
```

Casi d'uso comuni:

- Formattazione automatica
- Impostazioni specifiche per la lingua
- Evidenziazione personalizzata
- Rilevamento del tipo di file
- Gestione dell'area di lavoro e del progetto

## Comandi della configurazione

### `checktime`

Lo scopo di questo autocomando è controllare automaticamente il *timestamp* del buffer corrente e ricaricarlo se il file è stato modificato al di fuori di Neovim. Questo è utile quando si lavora su un file che viene modificato contemporaneamente da più utenti o processi, in quanto aiuta a mantenere il buffer di Neovim aggiornato con le ultime modifiche.

```lua
vim.api.nvim_create_autocmd({ "FocusGained", "TermClose", "TermLeave" }, {
 group = vim.api.nvim_create_augroup("checktime", { clear = true }),
 callback = function()
  if vim.o.buftype ~= "c" then
   vim.cmd("checktime")
  end
 end,
})
```

Il codice crea un gruppo di autocomandi chiamato **checktime** utilizzando ==vim.api.nvim_create_augroup()==, al quale viene passata l'opzione =={ clear = true }= che cancella il gruppo se presente prima di creare il nuovo autocomando.

La funzione ==vim.api.nvim_create_autocmd()== è utilizzata per creare un autocomando che reagisce a tre eventi:

- **FocusGained**: Questo evento viene attivato quando la finestra di Neovim riprende il focus, ad esempio quando l'utente passa nuovamente alla finestra di Neovim.
- **TermClose**: Questo evento si attiva quando viene chiuso il buffer di un terminale.
- **TermLeave**: Questo evento si attiva quando l'utente lascia un buffer di terminale.

La funzione di *callback* viene eseguita ogni volta che si verificano uno degli eventi specificati. All'interno della *callback*, il codice controlla se il buffer corrente non è un buffer terminale ==vim.o.buftype ~= “c”==, questo viene fatto per evitare di eseguire il comando sui buffer del terminale, in quanto potrebbe non essere necessario o desiderabile in quel contesto.  
Se non è un buffer terminale, il codice esegue il comando *checktime* usando ==vim.cmd(“checktime”)==. Questo comando ricarica il buffer, assicurando che qualsiasi modifica apportata al file al di fuori di Neovim si rifletta nel buffer.

### `close_with_q`

Questo codice è utile per creare un'esperienza di modifica più snella e mirata, soprattutto quando si lavora con buffer temporanei o informativi che non devono ingombrare l'elenco dei buffer.

```lua
vim.api.nvim_create_autocmd("FileType", {
 group = vim.api.nvim_create_augroup("close_with_q", { clear = true }),
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

Come nel caso precedente il codice crea un autocomando in ascolto dell'evento questa volta “FileType”, e crea un nuovo gruppo di autocomandi chiamato “close_with_q”.  
Il parametro ==pattern== specifica i tipi di file a cui applicare questo autocomando. In questo caso, include i file usati tipicamente per scopi informativi o temporanei, come i file di aiuto, le finestre di correzione rapida e i pannelli diagnostici.  
La funzione di callback viene eseguita ogni volta che viene attivato l'evento *FileType* per uno dei tipi di file specificati, la riga ==vim.bo[event.buf].buflisted = false== imposta l'opzione *buflisted* per il buffer corrente su ==false== nascondendo in questo modo il buffer dall'elenco dei buffer.  
La funzione ==vim.keymap.set== viene utilizzata per creare una nuova mappa dei tasti in modalità normale che chiude il buffer corrente quando viene premuto il tasto ++"q"++. Le opzioni =={ buffer = event.buf, silent = true }== assicurano che la *keymap* sia attiva solo per il buffer corrente e che il comando venga eseguito in modo silenzioso (senza alcun output).

### `highlight_yank`

Lo scopo di questo codice è quello di fornire un'indicazione visiva del testo che è stato selezionato. Quando si copia del testo in Neovim, il testo selezionato viene temporaneamente evidenziato, rendendo più facile vedere ciò che è stato copiato.  
Nel complesso, questo codice è un modo semplice ed efficace per migliorare l'esperienza di editing di Neovim, fornendo un segnale visivo per il testo copiato.

```lua
vim.api.nvim_create_autocmd("TextYankPost", {
    group = vim.api.nvim_create_augroup("highlight_yank", { clear = true }),
    callback = function()
        vim.highlight.on_yank()
    end,
})
```

L'autocomando crea un gruppo di autocomandi chiamato “highlight_yank” che viene attivato ogni volta che si verifica l'evento “TextYankPost”, evento che si verifica dopo ogni operazione di copia (*yank*) di una selezione di testo, viene attivato dopo che il testo è stato copiato, consentendoci di eseguire l'evidenziazione.

La funzione di callback viene eseguita quando si verifica l'evento “TextYankPost”. All'interno della funzione, viene richiamata la funzione ==vim.highlight.on_yank()==, che evidenzia il testo selezionato (*yanked*).

### `vertical_help`

Questo codice assicura che quando si apre un buffer di aiuto, questo venga visualizzato in una finestra verticale sul lato destro dello schermo e che le dimensioni della finestra vengano adattate al contenuto.

```lua
vim.api.nvim_create_autocmd("FileType", {
 group = vim.api.nvim_create_augroup("vertical_help", { clear = true }),
 pattern = "help",
 callback = function()
  vim.bo.bufhidden = "unload"
  vim.cmd.wincmd("L")
  vim.cmd.wincmd("=")
 end,
})
```

Il codice crea un gruppo chiamato “vertical_help” e crea un autocomando che si attiva quando il file file è di tipo ==help==.

All'interno della funzione di callback dell'autocomando troviamo:

- L'opzione **vim.bo.bufhidden** impostata su ==unload== per il buffer corrente. Questo assicura che il buffer di aiuto venga scaricato quando la finestra viene chiusa, invece di essere nascosto.
- Il comando **vim.cmd.wincmd(“L”)** sposta la finestra corrente nella posizione più a destra dello schermo.
- Il comando **vim.cmd.wincmd(“=”)** equalizza la larghezza di tutte le finestre, assicurando che la finestra di aiuto occupi la massima larghezza disponibile.

### `term_spell_off`

Lo scopo di questo codice è quello di disabilitare automaticamente la funzione di controllo ortografico ogni volta che viene aperto un buffer di terminale nell'editor Neovim. Ciò può essere utile se non si vuole che il controllo ortografico interferisca con i flussi di lavoro basati sul terminale.

```lua
vim.api.nvim_create_autocmd({ "TermOpen" }, {
 group = vim.api.nvim_create_augroup("term_spell_off", { clear = true }),
 callback = function()
  vim.wo.spell = false
 end,
})
```

Il codice crea un gruppo di autocomandi chiamato “term_spell_off” e successivamente crea un autocomando che ascolta l'evento “TermOpen”.

La funzione di callback viene attivata quando si presenta l'evento “TermOpen”. In questo caso, il callback imposta l'opzione ortografia su ==false== per la finestra corrente (*vim.wo.spell = false*). Questo disabilita la funzione di controllo ortografico nel buffer del terminale.

L'evento “TermOpen” è un evento specifico di Neovim che si attiva quando viene aperto un nuovo buffer di terminale. Può essere utile per impostare configurazioni specifiche del terminale, come la disabilitazione del controllo ortografico, come mostrato nell'esempio.

## Conclusioni

Gli autocomandi in Neovim sono una potente funzione che consente di personalizzare il comportamento di Neovim eseguendo automaticamente funzioni o comandi quando si verificano eventi specifici. Possono essere utilizzati per automatizzare varie attività, come la formattazione del codice, l'impostazione dei tipi di file o il ridimensionamento delle finestre, rendendo la configurazione di Neovim più efficiente e adatta alle proprie esigenze.
