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
## Introduzione a mappings.lua

Il file mappings.lua rappresenta un elemento fondamentale nella configurazione di Neovim, in quanto contiene una serie di impostazioni personalizzate per le combinazioni di tasti e le azioni associate. All'interno di mappings.lua, sono definite numerose combinazioni di tasti che attivano funzioni specifiche, come ad esempio la salvataggio del buffer corrente, la creazione di un nuovo buffer, la chiusura del buffer attuale o di tutti i buffer aperti. Inoltre, sono presenti anche mapping per l'accesso a funzionalità avanzate come la ricerca e la sostituzione di testo, la gestione dei buffer e la navigazione all'interno dei file.

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

Un esempio è il seguente:

```lua
-- conform - manual formatting
editor.remap("n", "<leader>F", function()
	require("conform").format({ lsp_fallback = true })
end, editor.make_opt("format buffer"))
```

In questo caso la variabile editor viene utilizzata per accedere a due funzioni diverse:

- `remap` per creare una nuova mappatura definita per la modalità normale ("n"), e la combinazione di tasti <leader>F. Quando viene premuta questa combinazione, viene eseguita la formattazione del buffer corrente.

- `make_opt` per creare un oggetto di opzioni per la mappatura di tasti. In questo caso, alle opzioni sopra citate (silent, noremap, e desc) viene aggiunto "format buffer" per fornire una breve spiegazione dell'azione eseguita dalla mappatura.

### Mappature per i buffer

Il file mappings.lua fornisce diverse mappature dei buffer che consentono di gestire i buffer in Neovim in modo efficiente. Queste mappature sono definite nella sezione *Buffer mappings* e sono elencate di seguito:

- Salvare il buffer corrente: `<C-s>` Questa mappatura salva il buffer corrente quando si preme ++ctrl+"s"++ in modalità di inserimento o normale.
- Creare un nuovo buffer: `<leader>b` Questa mappatura crea un nuovo buffer quando si preme ++space+"b"++ in modalità normale.
- Chiudere il buffer corrente: `<leader>x` Questa mappatura chiude il buffer corrente quando si preme ++space+"x"++ in modalità normale.
- Chiudere tutti i buffer: `<leader>X` Questa mappatura chiude tutti i buffer quando si preme ++space+"X"++ in modalità normale.

Queste mappature consentono di gestire i buffer in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile salvare il buffer corrente senza dover digitare :w e premere Invio.
