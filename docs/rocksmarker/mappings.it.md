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

Il file `lua/mapping.lua` è un componente fondamentale nella configurazione di Neovim. Questo file contiene le definizioni delle mappature di tastiera, ovvero le associazioni tra le combinazioni di tasti e le azioni che devono essere eseguite quando queste vengono premute.

Le mappature di tastiera sono essenziali per migliorare la produttività e l'efficienza nel lavoro con Neovim. Consentono di eseguire operazioni complesse con pochi tasti, riducendo il tempo e lo sforzo necessari per completare le attività.  
Inoltre, le mappature di tastiera possono essere personalizzate per adattarsi alle esigenze specifiche dell'utente, rendendo Neovim un ambiente di lavoro ancora più confortevole e intuitivo.

## Mappatura in Rocksmaker

Per semplificare ulteriormente la scrittura delle mappature in Rocksmarker sono fornite alcune funzioni dedicate presenti nel file `lua/utils/editor.lua`. Lo scopo del codice è quello di definire le funzioni per la gestione delle mappature dei tasti all'interno dell'editor. La funzione **M.set_key_mapping** in particolare costituisce l'interfaccia principale per l'impostazione di nuove mappature di tasti.  
Le funzioni definite inoltre consentono di superare la limitazione di quattro componenti che normalmente limita l'uso di **vim.keymap.set**, fornendo un modo più flessibile e organizzato per configurare le mappature dei tasti in Neovim.

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

- `remap` per creare una nuova mappatura definita per la modalità normale ("n"), e la combinazione di tasti `<leader>F`. Quando viene premuta questa combinazione, viene eseguita la formattazione del buffer corrente.

- `make_opt` per creare un oggetto di opzioni per la mappatura dei tasti. In questo caso, alle opzioni sopra citate *silent* e *noremap* viene aggiunto "format buffer" come *desc* per fornire una breve spiegazione dell'azione eseguita dalla mappatura.

### Gestione dei buffer

Le mappature per i buffer definite in questa sezione rappresentano un insieme di comandi personalizzati per gestire i buffer nel proprio ambiente di lavoro. Queste mappature migliorano l'efficienza e la produttività nell'editing dei file, consentendo di gestire i buffer in modo più intuitivo e personalizzato.

: **Salvare il buffer corrente**: `<C-s>`

: Salva il buffer corrente quando si preme ++ctrl+"s"++ in modalità di inserimento o normale.

: **Creare un nuovo buffer**: `<leader>b`

: Crea un nuovo buffer quando si preme ++space+"b"++ in modalità normale.

: **Chiudere il buffer corrente**: `<leader>x`

: Chiude il buffer corrente quando si preme ++space+"x"++ in modalità normale.

: **Chiudere tutti i buffer**: `<leader>X`

: Chiude tutti i buffer quando si preme ++space+"X"++ in modalità normale.

!!! tip "Uso di `<leader>X`"

    La chiusura di tutti i buffer aperti nell'editor con un solo comando risulta molto utile quando si lavora su più progetti e si desidera cambiare progetto senza uscire dall'editor visto che il plugin **persisted.nvim** non gestisce questo aspetto. Quindi per evitare di avere nella nuova sessione anche i file aperti nella precedente è possibile rimuoverli in una sola operazione prima di cambiare sessione.

Queste mappature consentono di gestire i buffer in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile salvare il buffer corrente senza dover digitare ++colon+"w"++ e premere ++enter++.

### Comandi dell'Editor

Le mappature definite nella sezione rappresentano un insieme di comandi personalizzati per migliorare l'esperienza di editing nel proprio ambiente di lavoro. Queste mappature consentono di eseguire operazioni come la formattazione del testo, la gestione dei buffer, la ricerca e la sostituzione di testo, nonché l'accesso a funzionalità avanzate come la visualizzazione della diagnostica e la gestione dei simboli.

: **Quit editor**: `<leader>q`

: Consente di uscire dall'editor corrente quando si preme ++space+"q"++ in modalità normale.

: **Clear highlights**: `<Esc>`

: Cancella gli highlight di ricerca quando si preme ++esc++ in modalità normale.

: **Telescope cmdline**: `,` (virgola)

: Apre la linea di comando in un buffer di Telescope quando si preme ++comma++ in modalità normale.

: **Formattazione del buffer**: `<leader>F`

: Formatta il buffer corrente quando si preme ++space+"F"++ in modalità normale.

: **Copia testo selezionato**: `<C-c>` (in modalità visuale)

: Copia il testo selezionato nella clipboard di sistema quando si preme ++ctrl+c++ in modalità visuale.

: **Taglia testo selezionato**: `<C-x>` (in modalità visuale)

: Taglia il testo selezionato e lo copia nella clipboard di sistema quando si preme ++ctrl+x++ in modalità visuale.

: **Copia riga intera**: `<S-c>` (in modalità normale)

: Copia la riga intera nella clipboard di sistema quando si preme ++shift+"c"++ in modalità normale.

: **Taglia riga intera**: `<S-x>` (in modalità normale)

: Taglia la riga intera e la copia nella clipboard di sistema quando si preme ++shift+"x"++ in modalità normale.

: **Incolla testo**: `<C-v>` (in modalità normale e visuale)

: Incolla il testo dalla clipboard di sistema quando si preme ++ctrl+"v"++ in modalità normale o visuale.

: **Incolla testo**: `<C-v>` (in modalità di inserimento)

: Incolla il testo dalla clipboard di sistema quando si preme ++ctrl+"v"++ in modalità di inserimento.

: **Incolla senza sovrascrivere il registro**: `<CS-v>` (in modalità visuale)

: Incolla il testo senza sovrascrivere il registro quando si preme ++ctrl+shift+"v"++ in modalità visuale.

: **Spostamento del blocco di testo**: `J`e `K` (in modalità visuale)

: Spostano il blocco di testo selezionato verso l'alto o verso il basso quando si preme ++"J"++ o ++"K"++ in modalità visuale. Per selezionare il blocco da spostare usare la mappatura standard di Neovim ++shift+"V"++.

: **Toggle diagnostica**: `<leader>dd`

: Attiva o disattiva il testo virtuale della diagnostica nel buffer quando si preme ++space+"d"+"d"++ in modalità normale.

### Gestione dei file

Queste mappature personalizzate aiutano a sfruttare al meglio le funzionalità di Neo-tree, migliorando la produttività e semplificando la gestione dei file e delle directory all'interno dell'editor. Ciò consente di lavorare in modo più efficiente e organizzato, con una visibilità completa sulla struttura dei file e delle directory del proprio progetto.

: **Apri Neo-Tree in floating window**: `.` (comma)

: Apre e chiude Neo-Tree in una finestra flottante quando si preme il ++"."++ in modalità normale.

: **Apri Neo-Tree in finestra a destra**: `<C-n>`

: Apre e chiude Neo-Tree in una finestra a destra quando si preme ++ctrl+"n"++ in modalità normale.

: **Rivela file corrente in Neo-Tree**: `<leader>fr`

: Rivela il file corrente in Neo-Tree quando si preme ++space+"f"+"r"++ in modalità normale.

Queste mappature consentono di accedere rapidamente alle funzionalità di Neo-Tree e di gestire il file system in Neovim in modo efficiente. Ad esempio, è possibile aprire Neo-Tree in una finestra galleggiante senza dover utilizzare il comando standard `:Neotree float`.

### Bufferline

Queste mappature aiutano a gestire i buffer in modo più rapido e intuitivo, riducendo il tempo speso nella navigazione e nella gestione dei buffer e consentendo di concentrarsi maggiormente sul lavoro di editing e sviluppo. Ciò migliora la produttività e la gestione dello spazio di lavoro, rendendo l'editor più efficiente e facile da utilizzare.

: **Seleziona buffer**: `<leader>bp`

: Permette la selezione dei buffer quando si preme ++space+"b"+"p"++ in modalità normale. Alla digitazione di questa chiave il logo del tipo di buffer viene sostituito da un carattere evidenziato in rosso nella bufferline e la digitazione della lettera corrispondente porta il focus sul buffer corrispondente.

: **Chiudi buffer selezionato**: `<leader>bc`

: Consente di chiudere un buffer aperto quando si preme ++space+"b"+"c"++ in modalità normale. Come per la selezione viene sostituito il logo del buffer e alla digitazione del carattere corrispondente viene chiuso il buffer.

: **Cicla al buffer successivo**: `<TAB>`

: Passa al buffer successivo quando si preme il tasto ++tab++ in modalità normale.

: **Cicla al buffer precedente**: `<S-TAB>`

: Passa al buffer precedente quando si preme ++shift+tab++ in modalità normale.

Queste mappature consentono di gestire i buffer in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile passare al buffer successivo o precedente senza dover digitare `:bnext` o `:bprevious`.

### Ricerca e anteprima

Queste mappature personalizzate aiutano a sfruttare al meglio le funzionalità di Telescope, migliorando la gestione dei file e della navigazione all'interno dell'editor.

: **Elenco dei buffe**r: `<leader>fb`

: Apre la finestra di Telescope con l'elenco dei buffer aperti quando si preme ++space+"f"+"b"++ in modalità normale.

: **Ricerca di file**: `<leader>ff`

: Apre la finestra di Telescope per la ricerca di file nella directory corrente quando si preme ++space+"f"+"f"++ in modalità normale.

: **Ricerca di file recenti**: `<leader>fo`

: Apre la finestra di Telescope con l'elenco dei file recentemente aperti quando si preme ++space+"f"+"o"++ in modalità normale.

: **Ricerca di file frequenti**: `<leader>fc`

: Apre la finestra di Telescope con l'elenco dei file più frequentemente utilizzati quando si preme ++space+"f"+"c"++ in modalità normale.

: **Ricerca all'interno del buffer corrente**: `<Leader>fz`

: Apre la finestra di Telescope per la ricerca all'interno del buffer corrente quando si preme ++space+"f"+"z"++ in modalità normale.

Queste mappature consentono di eseguire ricerche e operazioni di file in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile aprire la finestra di Telescope per la ricerca degli ultimi file modificati senza dover digitare `:Telescope old_files`.

### Diagnostica

Le mappature definite in questa sezione rappresentano un insieme di comandi personalizzati per gestire le diagnostiche e i problemi nel proprio ambiente di lavoro, sono state progettate per aiutare gli utenti a identificare e risolvere i problemi nel loro codice in modo più efficiente e rapido, migliorando così la produttività e la qualità del lavoro.

: **Toggle diagnostica globale**: `<leader>dt`

: Attiva o disattiva la diagnostica globale quando si preme ++space+"d"+"t"++ in modalità normale.

: **Toggle diagnostica del buffer corrente**: `<leader>db`

: Attiva o disattiva la diagnostica del buffer corrente quando si preme ++space+"d"+"b"++ in modalità normale.

: **Toggle simboli del buffer corrente**: `<leader>ds`

: Attiva o disattiva la visualizzazione dei simboli del buffer corrente quando si preme ++space+"d"+"s"++ in modalità normale. Questa funzionalità applicata al codice Markdown permette di navigare tra le intestazioni del file riducendo il tempo per il posizionamento nel punto desiderato.

Queste mappature consentono di gestire gli errori e le diagnostiche in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile attivare o disattivare la diagnostica globale senza dover digitare `:TroubleToggle`.

### Sessioni

Queste mappature personalizzate aiutano a gestire le sessioni di lavoro in modo più efficiente, permettendo di riprendere il lavoro da dove si era interrotto e di mantenere lo stato dell'editor tra le diverse sessioni di lavoro. Ciò consente di lavorare in modo più organizzato e produttivo, con la possibilità di riprendere il lavoro in qualsiasi momento e di mantenere la continuità delle attività.

: **Seleziona sessione**: `<A-s>`

: Apre la finestra di selezione delle sessioni quando si preme ++alt+s++ in modalità normale.

: **Carica ultima sessione**: `<A-l>`

: Carica l'ultima sessione salvata quando si preme ++alt+"l"++ in modalità normale.

: **Seleziona sessione (alternativa)**: `<leader>sS`

: Apre la finestra di selezione delle sessioni quando si preme ++space+"s"+"S"++ in modalità normale.

: **Salva sessione corrente**: `<leader>ss`

: Salva la sessione corrente quando si preme ++space+"s"+"s"++ in modalità normale.

: **Carica ultima sessione (alternativa)**: `<leader>sl`

: Carica l'ultima sessione salvata quando si preme ++space+"s"+"l"++ in modalità normale.

: **Interrompi sessione corrente**: `<leader>st`

: Interrompe la sessione corrente quando si preme ++space+"s"+"t"++ in modalità normale.

Queste mappature consentono di gestire le sessioni in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile caricare l'ultima sessione salvata senza dover digitare `:SessionLoadLast`.

### Ricerca e Sostituzione

Queste mappature personalizzate aiutano a semplificare le operazioni di ricerca e sostituzione, riducendo il tempo speso nella ricerca di testo. Ciò consente di lavorare in modo più efficiente e preciso, con la possibilità di eseguire ricerche e sostituzioni in modo rapido e personalizzato, e di mantenere il controllo sul testo e sulla sua formattazione.

: **Attiva/disattiva Spectre**: `<leader>R`

: Attiva o disattiva la modalità di ricerca e sostituzione di Spectre quando si preme ++space+"R"++ in modalità normale.

: **Ricerca parola corrente**: `<leader>rw`

: Esegue una ricerca della parola corrente in tutti i file del progetto quando si preme ++space+"r"+"w"++ in modalità normale.

: **Ricerca parola corrente nel file**: `<leader>rp`

: Esegue una ricerca della parola corrente nel file corrente quando si preme ++space+"r"+"p"++ in modalità normale.

**Nvim-spectre** consente di eseguire ricerche di testo nel file corrente o in tutti i file del progetto, sostituire il testo trovato con un nuovo testo e selezionare la modalità di ricerca, ad esempio ricerca di parole intere o ricerca di espressioni regolari.  
Inoltre offre altre funzionalità avanzate, come ad esempio la possibilità di eseguire ricerche in più file contemporaneamente, di eseguire ricerche in directory e sottodirectory ed escludere file o directory dalla ricerca.

### Differenze

Queste mappature sono state progettate per tenere traccia delle modifiche apportate ai file e a gestire le versioni del codice in modo più efficiente e rapido, migliorando la collaborazione e la gestione del codice. Con **diffview.nvim** è possibile lavorare in modo più trasparente e sicuro, conoscendo sempre le modifiche apportate ai file e le versioni del codice utilizzate.

: **Apri Diffview**: `<leader>dv`

: Apre la finestra di Diffview per visualizzare le differenze tra il file corrente e la versione precedente quando si preme ++space+"d"+"v"++ in modalità normale.

: **Apri storia del file**: `<leader>dh`

: Apre la finestra di Diffview per visualizzare la cronologia del file corrente quando si preme ++space+"d"+"h"++ in modalità normale.

: **Apri storia del buffer**: `<leader>df`

: Apre la finestra di Diffview per visualizzare la cronologia del buffer corrente quando si preme ++space+"d"+"f"++ in modalità normale.

: **Chiudi Diffview**: `<leader>dc`

: Chiude la finestra di Diffview quando si preme ++space+"d"+"c"++ in modalità normale. Diffview fornisce un comando dedicato per la chiusura in quanto la mappatura per la sua chiusura risulta particolarmente complicata da realizzare con le mappature di Neovim.

: **Confronta con HEAD**: `<leader>dH`

: Apre la finestra di Diffview per confrontare il file corrente con la versione HEAD quando si preme ++space+"d"+"H"++ in modalità normale.

Queste mappature consentono di visualizzare le differenze tra file e buffer in Neovim in modo rapido e efficiente, senza dover utilizzare i comandi di Neovim standard. Ad esempio, è possibile aprire la finestra di Diffview per visualizzare le differenze tra il file corrente e la versione precedente senza dover digitare `:DiffviewOpen`.

### Repository Git

Queste mappature consentono di eseguire operazioni come aprire la gestione di git per il workspace o per il buffer corrente, visualizzare la storia dei commit e accedere alle funzionalità avanzate di git, sono state progettate per aiutare gli utenti a gestire il versionamento del codice in modo più efficiente e rapido.

: **Apri Neogit**: `<leader>gm`

: Apre Neogit, un'interfaccia utente per Git, quando si preme ++space+"g"+"m"++ in modalità normale.

: **Apri Neogit per il buffer corrente**: `<leader>gM`

: Apre Neogit per il buffer corrente quando si preme ++space+"g"+"M"++ in modalità normale.  
Questo consente di applicare modifiche a file esterni al workspace corrente senza dover abbandonare la sessione.

: **Visualizza la storia dei commit**: `<leader>gh`

: Visualizza la storia dei commit del repository quando si preme ++space+"g"+"h"++ in modalità normale.

: **Visualizza la storia dei commit del buffer corrente**: `<leader>gb`

: Visualizza la storia dei commit del buffer corrente quando si preme ++space+"g"+"b"++ in modalità normale.

La mappatura di Git consente di gestire i repository Git direttamente all'interno di Neovim, visualizzare la storia dei commit dei repository e dei buffer ed eseguire comandi Git direttamente da Neovim.

### Terminale

Le mappature per il terminale definite in questa sezione rappresentano un insieme di comandi personalizzati per gestire la finestra del terminale nel proprio ambiente di lavoro. Con queste mappature, gli utenti possono lavorare in modo più integrato con il terminale, eseguendo operazioni di sistema e di gestione del codice in modo più semplice e veloce.

: Toggle Terminale Orizzontale: `<a-t>`

: Consente di aprire e chiudere il terminale in modalità orizzontale. Quando si preme ++alt+"t"++ in modalità normale, inserimento o terminale, il terminale si apre o chiude in modalità orizzontale.

: Toggle Terminale Verticale: `<a-v>`

: Consente di aprire e chiudere il terminale in modalità verticale. Quando si preme ++alt+"v"++ in modalità normale, inserimento o terminale, il terminale si apre o chiude in modalità verticale.

: Toggle Terminale Float: `<a-f>`

: Consente di aprire e chiudere il terminale in modalità float. Quando si preme ++alt+"f"++ in modalità normale, inserimento o terminale, il terminale si apre o chiude in modalità float.

Supponendo di voler eseguire un comando del sistema operativo all'interno di Neovim è possibile utilizzare la mappatura per aprire il terminale in modalità orizzontale. Premendo ++alt+"t"++ in modalità normale e il terminale si apre in modalità orizzontale.

### Gestione dei Logs

La mappatura definita in questa sezione serve a gestire le notifiche e i messaggi di stato nel proprio ambiente di lavoro. Questa mappatura consente di visualizzare le notifiche e i messaggi di stato dei processi in esecuzione, nonché di gestire le informazioni di stato dei plugin e delle estensioni.

: Visualizza messaggi: `<leader>lg`

: Consente di visualizzare i messaggi di sistema passati a **fidget.nvim**. Quando si preme ++space+"l"+"g"++ in modalità normale, si apre una finestra con i messaggi di stato dell'editor e dei plugin.

## Conclusioni

In conclusione, le mappature presentate in questo documento rappresentano una raccolta completa e personalizzata di comandi e scorciatoie per migliorare l'esperienza di utilizzo dell'editor di codice. La funzione `editor.remap()` è stata utilizzata in modo estensivo per costruire queste mappature, offrendo una grande flessibilità e personalizzazione nella definizione di comandi personalizzati.

La funzione `editor.remap()` consente di ridefinire i comandi esistenti o di crearne di nuovi, assegnando loro azioni specifiche e descrizioni personalizzate. Ciò permette di creare una raccolta di mappature che coprono una vasta gamma di funzionalità, dalle operazioni di base come la salvataggio e la chiusura dei buffer, alle funzionalità più avanzate come la gestione dei file, la ricerca e la sostituzione del testo, e la gestione dei commit Git.
