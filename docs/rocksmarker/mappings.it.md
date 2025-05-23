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

Il file mappings.lua rappresenta un elemento fondamentale nella configurazione di Neovim, in quanto contiene una serie di impostazioni personalizzate per le combinazioni di tasti e le azioni associate. All'interno di mappings.lua, sono definite numerose combinazioni di tasti che attivano funzioni specifiche, come ad esempio la salvataggio del buffer corrente, la creazione di un nuovo buffer, la chiusura del buffer attuale o di tutti i buffer aperti.  
Inoltre, sono presenti anche mappature per l'accesso a funzionalità avanzate come la ricerca e la sostituzione di testo, la gestione dei buffer e la navigazione all'interno dei file.

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

### Mappature dell'Editor

Il file mappings.lua fornisce diverse mappature dell'editor che consentono di gestire l'editor in Neovim in modo efficiente. Queste mappature sono definite nella sezione Editor mappings e sono elencate di seguito:

#### Comandi di base

- Quit editor: `<leader>q` consente di uscire dall'editor corrente quando si preme ++space+"q"++ in modalità normale.
- Clear highlights: `<Esc>` cancella gli highlight di ricerca quando si preme ++esc++ in modalità normale.
- Telescope cmdline: `,` (virgola) apre la linea di comando in un buffer di Telescope quando si preme ++comma++ in modalità normale.

#### Formattazione e copia

- Formattazione del buffer: `<leader>F` formatta il buffer corrente quando si preme ++space+"F"++ in modalità normale.
- Copia testo selezionato: `<C-c>` (in modalità visuale) copia il testo selezionato nella clipboard di sistema quando si preme ++ctrl+c++ in modalità visuale.
- Taglia testo selezionato: `<C-x>` (in modalità visuale) taglia il testo selezionato e lo copia nella clipboard di sistema quando si preme ++ctrl+x++ in modalità visuale.
- Copia riga intera: `<S-c>` (in modalità normale) copia la riga intera nella clipboard di sistema quando si preme ++shift+"c"++ in modalità normale.
- Taglia riga intera: `<S-x>` (in modalità normale) taglia la riga intera e la copia nella clipboard di sistema quando si preme ++shift+"x"++ in modalità normale.

#### Incolla e sostituisci

- Incolla testo: `<C-v>` (in modalità normale e visuale) incolla il testo dalla clipboard di sistema quando si preme ++ctrl+"v"++ in modalità normale o visuale.
- Incolla testo in modalità di inserimento: `<C-v>` (in modalità di inserimento) incolla il testo dalla clipboard di sistema quando si preme ++ctrl+"v"++ in modalità di inserimento.
- Incolla senza sovrascrivere il registro: `<S-v>` (in modalità visuale) incolla il testo senza sovrascrivere il registro quando si preme ++shift+"v"++ in modalità visuale.

#### Altri comandi

- Spostamento del blocco di testo: `J`e `K` (in modalità visuale) spostano il blocco di testo selezionato verso l'alto o verso il basso quando si preme ++"J"++ o ++"K"++ in modalità visuale.
- Toggle diagnostica: `<leader>dd` attiva o disattiva il testo virtuale della diagnostica nel buffer quando si preme ++space+"d"+"d"++ in modalità normale.

### Mappature di Neo-Tree

Il file mappings.lua fornisce diverse mappature per Neo-Tree che consentono di gestire il file system in Neovim in modo efficiente. Queste mappature sono definite nella sezione neo-tree.nvim mappings e sono elencate di seguito:

#### Comandi di base

- Apri Neo-Tree in floating window: `.` apre e chiude Neo-Tree in una finestra flottante quando si preme il ++"."++ in modalità normale.
- Apri Neo-Tree in finestra a destra: `<C-n>` apre e chiude Neo-Tree in una finestra a destra quando si preme ++ctrl+"n"++ in modalità normale.

#### Comandi di navigazione

- Rivela file corrente in Neo-Tree: `<leader>fr` Questa mappatura rivela il file corrente in Neo-Tree quando si preme ++space+"f"+"r"++ in modalità normale.

Queste mappature consentono di accedere rapidamente alle funzionalità di Neo-Tree e di gestire il file system in Neovim in modo efficiente. Ad esempio, è possibile aprire Neo-Tree in una finestra galleggiante senza dover utilizzare i comandi di Neovim standard.

### Mappature di Bufferline

Il file mappings.lua fornisce diverse mappature per bufferline.nvim che consentono di gestire i buffer in Neovim in modo efficiente:

#### Comandi di navigazione

- Seleziona buffer: `<leader>bp` apre la finestra di selezione dei buffer quando si preme ++space+"b"+"p"++ in modalità normale.
 Chiudi buffer selezionato: `<leader>bc` chiude il buffer selezionato quando si preme ++space+"b"+"c"++ in modalità normale.

#### Comandi di ciclazione

- Cicla al buffer successivo: `<TAB>` passa al buffer successivo quando si preme il tasto ++tab++ in modalità normale.
- Cicla al buffer precedente: `<S-TAB>` passa al buffer precedente quando si preme ++shift+tab++ in modalità normale.

Queste mappature consentono di gestire i buffer in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile passare al buffer successivo o precedente senza dover digitare `:bnext` o `:bprevious`.

### Mappature di Telescope

Il file mappings.lua fornisce diverse mappature per Telescope che consentono di eseguire ricerche e operazioni di file in Neovim in modo efficiente. Queste mappature sono definite nella sezione telescope.nvim mappings e sono elencate di seguito:
Comandi di ricerca

- Elenco dei buffer: `<leader>fb` Questa mappatura apre la finestra di Telescope con l'elenco dei buffer aperti quando si preme ++space+"f"+"b"++ in modalità normale.
- Ricerca di file: `<leader>ff` Questa mappatura apre la finestra di Telescope per la ricerca di file nella directory corrente quando si preme ++space+"f"+"f"++ in modalità normale.
- Ricerca di file recenti: `<leader>fo` Questa mappatura apre la finestra di Telescope con l'elenco dei file recentemente aperti quando si preme ++space+"f"+"o"++ in modalità normale.
- Ricerca di file frequenti: `<leader>fc` Questa mappatura apre la finestra di Telescope con l'elenco dei file più frequentemente utilizzati quando si preme ++space+"f"+"c"++ in modalità normale.
- Ricerca all'interno del buffer corrente: `<Leader>fz` Questa mappatura apre la finestra di Telescope per la ricerca all'interno del buffer corrente quando si preme ++space+"f"+"z"++ in modalità normale.

Queste mappature consentono di eseguire ricerche e operazioni di file in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile aprire la finestra di Telescope per la ricerca di file senza dover digitare `:Telescope find_files`.

### Mappature di Trouble

Il file mappings.lua fornisce diverse mappature per trouble.nvim che consentono di gestire gli errori e le diagnostiche in Neovim in modo efficiente:

#### Comandi di diagnostica

- Toggle diagnostica globale: `<leader>dt` Questa mappatura attiva o disattiva la diagnostica globale quando si preme ++space+"d"+"t"++ in modalità normale.
- Toggle diagnostica del buffer corrente: `<leader>db` Questa mappatura attiva o disattiva la diagnostica del buffer corrente quando si preme ++space+"d"+"b"++ in modalità normale.
- Toggle simboli del buffer corrente: `<leader>ds` Questa mappatura attiva o disattiva la visualizzazione dei simboli del buffer corrente quando si preme ++space+"d"+"s"++ in modalità normale.

Queste mappature consentono di gestire gli errori e le diagnostiche in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile attivare o disattivare la diagnostica globale senza dover digitare `:TroubleToggle`.

### Mappature di persisted.nvim

Il file mappings.lua fornisce diverse mappature per la gestione delle sessioni in Neovim utilizzando il plugin persisted.nvim:

#### Comandi di sessione

- Seleziona sessione: `<A-s>` apre la finestra di selezione delle sessioni quando si preme Alt+s in modalità normale.
- Carica ultima sessione: `<A-l>` carica l'ultima sessione salvata quando si preme ++alt+"l"++ in modalità normale.
- Seleziona sessione (alternativa): `<leader>sS` apre la finestra di selezione delle sessioni quando si preme ++space+"s"+"S"++ in modalità normale.
- Salva sessione corrente: `<leader>ss` salva la sessione corrente quando si preme Space+ss in modalità normale.
- Carica ultima sessione (alternativa): `<leader>sl` carica l'ultima sessione salvata quando si preme ++space+"s"+"l"++ in modalità normale.
- Interrompi sessione corrente: `<leader>st` interrompe la sessione corrente quando si preme ++space+"s"+"t"++ in modalità normale.

Queste mappature consentono di gestire le sessioni in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile caricare l'ultima sessione salvata senza dover digitare `:Persisted load`.

### Ricerca e Sostituzione - Nvim-Spectre

Il file mappings.lua fornisce diverse mappature per la ricerca e la sostituzione di testo in Neovim utilizzando il plugin Nvim-Spectre. Queste mappature sono definite nella sezione search and replace - nvim-spectre e sono elencate di seguito:
Comandi di ricerca e sostituzione

    Attiva/disattiva Spectre: <leader>R Questa mappatura attiva o disattiva la modalità di ricerca e sostituzione di Spectre quando si preme Space+R in modalità normale.
    Ricerca parola corrente: <leader>rw Questa mappatura esegue una ricerca della parola corrente nel file quando si preme Space+rw in modalità normale.
    Ricerca parola corrente nel file: <leader>rp Questa mappatura esegue una ricerca della parola corrente nel file corrente quando si preme Space+rp in modalità normale.

Queste mappature consentono di eseguire ricerche e sostituzioni di testo in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile eseguire una ricerca della parola corrente nel file senza dover digitare :Spectre search.

La mappatura di Nvim-Spectre consente di utilizzare la seguente funzionalità:

    Ricerca di testo: è possibile eseguire ricerche di testo nel file corrente o in tutti i file del progetto.
    Sostituzione di testo: è possibile sostituire il testo trovato con un nuovo testo.
    Modalità di ricerca: è possibile selezionare la modalità di ricerca, ad esempio ricerca di parole intere o ricerca di espressioni regolari.

Inoltre, Nvim-Spectre offre molte altre funzionalità avanzate, come ad esempio:

    Ricerca in più file: è possibile eseguire ricerche in più file contemporaneamente.
    Ricerca in directory: è possibile eseguire ricerche in directory e sottodirectory.
    Esclusione di file: è possibile escludere file o directory dalla ricerca.

Ricerca e Sostituzione - Searchbox.nvim

Il file mappings.lua fornisce diverse mappature per la ricerca e la sostituzione di testo in Neovim utilizzando il plugin Searchbox.nvim. Queste mappature sono definite nella sezione search and replace - searchbox.nvim e sono elencate di seguito:
Comandi di ricerca e sostituzione

    Ricerca incrementale: <leader>si Questa mappatura esegue una ricerca incrementale del testo quando si preme Space+si in modalità normale.
    Ricerca di corrispondenze: <leader>sa Questa mappatura esegue una ricerca di corrispondenze del testo quando si preme Space+sa in modalità normale.
    Sostituzione di testo: <leader>sr Questa mappatura esegue una sostituzione di testo quando si preme Space+sr in modalità normale.

Queste mappature consentono di eseguire ricerche e sostituzioni di testo in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile eseguire una ricerca incrementale del testo senza dover digitare :Searchbox incsearch.

La mappatura di Searchbox.nvim consente di utilizzare la seguente funzionalità:

    Ricerca di testo: è possibile eseguire ricerche di testo nel file corrente o in tutti i file del progetto.
    Sostituzione di testo: è possibile sostituire il testo trovato con un nuovo testo.
    Modalità di ricerca: è possibile selezionare la modalità di ricerca, ad esempio ricerca di parole intere o ricerca di espressioni regolari.

Inoltre, Searchbox.nvim offre molte altre funzionalità avanzate, come ad esempio:

    Ricerca in più file: è possibile eseguire ricerche in più file contemporaneamente.
    Ricerca in directory: è possibile eseguire ricerche in directory e sottodirectory.
    Esclusione di file: è possibile escludere file o directory dalla ricerca.
    Opzioni di ricerca: è possibile personalizzare le opzioni di ricerca, ad esempio la sensibilità al caso o la ricerca di parole intere.

Searchbox.nvim è un plugin molto potente e flessibile per la ricerca e la sostituzione di testo in Neovim, e può essere utilizzato in combinazione con altri plugin per migliorare la produttività e l'efficienza dell'editor.

Mappature di Diffview

Il file mappings.lua fornisce diverse mappature per la visualizzazione delle differenze tra file in Neovim utilizzando il plugin Diffview.nvim. Queste mappature sono definite nella sezione diffview.nvim mappings e sono elencate di seguito:
Comandi di diff

    Apri Diffview: <leader>dv Questa mappatura apre la finestra di Diffview per visualizzare le differenze tra il file corrente e la versione precedente quando si preme Space+dv in modalità normale.
    Apri storia del file: <leader>dh Questa mappatura apre la finestra di Diffview per visualizzare la storia del file corrente quando si preme Space+dh in modalità normale.
    Apri storia del buffer: <leader>df Questa mappatura apre la finestra di Diffview per visualizzare la storia del buffer corrente quando si preme Space+df in modalità normale.
    Chiudi Diffview: <leader>dc Questa mappatura chiude la finestra di Diffview quando si preme Space+dc in modalità normale.
    Confronta con HEAD: <leader>dH Questa mappatura apre la finestra di Diffview per confrontare il file corrente con la versione HEAD quando si preme Space+dH in modalità normale.

Queste mappature consentono di visualizzare le differenze tra file e buffer in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile aprire la finestra di Diffview per visualizzare le differenze tra il file corrente e la versione precedente senza dover digitare :DiffviewOpen.

La mappatura di Diffview.nvim consente di utilizzare la seguente funzionalità:

    Visualizzazione delle differenze: è possibile visualizzare le differenze tra file e buffer in una finestra separata.
    Navigazione tra le differenze: è possibile navigare tra le differenze utilizzando i tasti di navigazione standard di Neovim.
    Confronto con versioni precedenti: è possibile confrontare il file corrente con versioni precedenti utilizzando la finestra di Diffview.

Inoltre, Diffview.nvim offre molte altre funzionalità avanzate, come ad esempio:

    Supporto per Git: è possibile utilizzare Diffview.nvim con Git per visualizzare le differenze tra commit e branch.
    Supporto per altri sistemi di controllo versione: è possibile utilizzare Diffview.nvim con altri sistemi di controllo versione, come ad esempio Mercurial e Subversion.
    Personalizzazione della finestra di Diffview: è possibile personalizzare la finestra di Diffview utilizzando le opzioni di configurazione di Neovim.

Mappature di Git

Il file mappings.lua fornisce diverse mappature per la gestione di Git in Neovim. Queste mappature sono definite nella sezione git mappings e sono elencate di seguito:
Comandi di Git

    Apri Neogit: <leader>gm Questa mappatura apre Neogit, un'interfaccia utente per Git, quando si preme Space+gm in modalità normale.
    Apri Neogit per il buffer corrente: <leader>gM Questa mappatura apre Neogit per il buffer corrente quando si preme Space+gM in modalità normale.
    Visualizza la storia dei commit: <leader>gh Questa mappatura visualizza la storia dei commit del repository quando si preme Space+gh in modalità normale.
    Visualizza la storia dei commit del buffer corrente: <leader>gb Questa mappatura visualizza la storia dei commit del buffer corrente quando si preme Space+gb in modalità normale.

Queste mappature consentono di gestire Git in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile aprire Neogit senza dover digitare :Neogit.

La mappatura di Git consente di utilizzare la seguente funzionalità:

    Gestione dei repository: è possibile gestire i repository Git direttamente all'interno di Neovim.
    Visualizzazione della storia dei commit: è possibile visualizzare la storia dei commit dei repository e dei buffer.
    Esecuzione di comandi Git: è possibile eseguire comandi Git direttamente all'interno di Neovim.

Mappature per il Terminale

Le mappature per il terminale sono fondamentali per utilizzare Neovim in modo efficiente. Il file mappings.lua fornisce diverse mappature per il terminale che consentono di gestire il terminale in modo comodo. Queste mappature sono definite nella sezione "Toggle terminal mappings" e sono elencate di seguito:
Mappature per il Terminale

Il terminale è uno strumento potente in Neovim che consente di eseguire comandi del sistema operativo all'interno dell'editor. Per utilizzare il terminale in modo efficiente, è necessario conoscere le mappature seguenti:

    Toggle Terminale Orizzontale: <a-t> Questa mappatura consente di aprire e chiudere il terminale in modalità orizzontale. Quando si preme Alt+t in modalità normale, inserimento o terminale, il terminale si apre o chiude in modalità orizzontale.
    Toggle Terminale Verticale: <a-v> Questa mappatura consente di aprire e chiudere il terminale in modalità verticale. Quando si preme Alt+v in modalità normale, inserimento o terminale, il terminale si apre o chiude in modalità verticale.
    Toggle Terminale Float: <a-f> Questa mappatura consente di aprire e chiudere il terminale in modalità float. Quando si preme Alt+f in modalità normale, inserimento o terminale, il terminale si apre o chiude in modalità float.
    Esci dalla Modalità Terminale: jk Questa mappatura consente di uscire dalla modalità terminale e tornare alla modalità normale. Quando si preme "jk" in modalità terminale, si esce dalla modalità terminale e si torna alla modalità normale.

Utilizzo delle Mappature del Terminale

Per utilizzare le mappature del terminale, è sufficiente premere la combinazione di tasti corrispondente alla mappatura desiderata. Ad esempio, per aprire il terminale in modalità orizzontale, è sufficiente premere Alt+t in modalità normale.
Esempio di Utilizzo

Supponiamo di voler eseguire un comando del sistema operativo all'interno di Neovim. Per fare ciò, possiamo utilizzare la mappatura per aprire il terminale in modalità orizzontale. Premiamo Alt+t in modalità normale e il terminale si apre in modalità orizzontale. A questo punto, possiamo eseguire il comando del sistema operativo desiderato.

In conclusione, le mappature per il terminale sono fondamentali per utilizzare Neovim in modo efficiente. Conoscere queste mappature consente di gestire il terminale in modo comodo e di eseguire comandi del sistema operativo all'interno dell

Mappature per Fidget.nvim

Fidget.nvim è un'estensione per Neovim che fornisce una visualizzazione dei messaggi di LSP (Language Server Protocol) in tempo reale. Per utilizzare Fidget.nvim in modo efficiente, è necessario conoscere la mappatura seguente:

    Visualizza Messaggi Fidget: <leader>lg Questa mappatura consente di visualizzare i messaggi di Fidget.nvim. Quando si preme Space+lg in modalità normale, si apre una finestra con i messaggi di Fidget.nvim.

Utilizzo delle Mappature di Fidget.nvim

Per utilizzare la mappatura di Fidget.nvim, è sufficiente premere la combinazione di tasti corrispondente alla mappatura desiderata. Ad esempio, per visualizzare i messaggi di Fidget.nvim, è sufficiente premere Space+lg in modalità normale.
Esempio di Utilizzo

Supponiamo di voler visualizzare i messaggi di LSP per il file corrente. Per fare ciò, possiamo utilizzare la mappatura per visualizzare i messaggi di Fidget.nvim. Premiamo Space+lg in modalità normale e si apre una finestra con i messaggi di Fidget.nvim. A questo punto, possiamo visualizzare i messaggi di LSP per il file corrente e diagnosticare eventuali problemi.
