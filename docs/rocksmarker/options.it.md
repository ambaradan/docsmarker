---
title: Options
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!--vale off-->
## Introduzione

Il file `lua/options.lua` consente di definire e personalizzare le opzioni di configurazione dell'editor in modo efficiente e organizzato, le opzioni definite in questo file influenzano direttamente l'esperienza dell'utente.

L'utilizzo del file *options.lua* consente una personalizzazione avanzata dell'editor senza la necessità di modificare il codice sorgente, di mantenere tutte le opzioni di configurazione in un unico file per facilitare la gestione e la manutenzione della configurazione.

## Options.lua in Rocksmarker

Il file **options.lua** nel progetto Rocksmarker è stato progettato per fornire una serie di impostazioni predefinite che migliorano l'esperienza di utilizzo di Neovim, coprendo aspetti come la gestione dei file, la formattazione del testo, la navigazione e la personalizzazione dell'interfaccia utente.

### Configurazione Globale

Questa sezione definisce variabili globali che influenzano il comportamento generale di Neovim.

: `vim.g.mapleader = vim.keycode("<Space>")`

: Imposta il tasto `<Space>`  come leader key. Il leader key è un prefisso utilizzato per definire mappature personalizzate (scorciatoie da tastiera). Ciò significa che tutte le mappature personalizzate definite nel file di configurazione inizieranno con la pressione del tasto ++space++.

: `vim.g.markdown_recommended_style = 0`

: Disabilita lo stile raccomandato per il Markdown. Questo potrebbe è fatto per evitare conflitti con lo stile preferito dal progetto Rocksmarker.

: `vim.g.loaded_python3_provider = 0`  
`vim.g.loaded_ruby_provider = 0`  
`vim.g.loaded_perl_provider = 0`  
`vim.g.loaded_node_provider = 0`

: Disabilitano i provider di linguaggio per Python, Ruby, Perl e Node.js. Questo è fatto per ridurre l'overhead di questi linguaggi che non sono necessari per il progetto, e per evitare avvisi prodotti dal comando `:checkhealth` relativi a dipendenze non soddisfatte per questi provider.

### Opzioni dell'Editor

Questa sezione configura il comportamento dell'editor di testo, influenzando la visualizzazione e l'interazione con i file.

: `vim.o.foldenable = true`  

: Abilita il folding del codice, permettendo di comprimere e espandere sezioni di codice. Questo può aiutare a ridurre la quantità di codice visibile e a concentrarsi sulle parti più rilevanti.

: `vim.o.foldmethod = "marker"`  

: Imposta il metodo di folding su "marker", il che significa che il folding è determinato da marcatori specifici nel codice. Questi marcatori possono essere inseriti manualmente dall'utente per definire le sezioni di codice da piegare.

: `vim.o.foldmarker = "{{{,}}}"`

: Definisce i marcatori utilizzati per delimitare le sezioni di codice da piegare. In questo caso, `{{{` segna l'inizio e `}}}` la fine di una sezione pieghevole.

: `vim.o.clipboard = "unnamedplus"`

: Configura l'integrazione con la clipboard di sistema (Linux). L'opzione "unnamedplus" permette di copiare e incollare direttamente con la clipboard del sistema operativo, senza la necessità di utilizzare comandi specifici di Vim.

: `vim.o.fillchars = "eob: "`

: Personalizza i caratteri di riempimento. In questo caso, nasconde il carattere ~ (tilde) per le righe vuote alla fine del buffer, migliorando la leggibilità del codice.

: `vim.o.timeoutlen = 400`

: Imposta il tempo di attesa (in millisecondi) dopo ogni tasto premuto prima di considerarlo parte di una sequenza di comandi. Questo influenza la velocità di esecuzione dei comandi e la sensibilità della tastiera.

: `vim.o.undofile = true`

: Abilita il salvataggio automatico della cronologia degli undo, permettendo di annullare modifiche anche dopo aver chiuso e riaperto il file. Ciò è particolarmente utile per recuperare versioni precedenti del codice.

: `vim.o.cursorline = true`

: Evidenzia la riga corrente dove si trova il cursore, aiutando a mantenere la concentrazione sulla posizione corrente all'interno del codice.

: `vim.o.ignorecase = true`

: Imposta la ricerca in modo da non considerare la differenza tra maiuscole e minuscole, semplificando la ricerca di parole chiave all'interno del codice.

: `vim.o.smartcase = true`

: Modifica il comportamento di `ignorecase` in modo che la ricerca diventi *case-sensitive* solo se si utilizzano maiuscole nel pattern di ricerca. Ciò offre un equilibrio tra facilità di ricerca e precisione.

### Impostazioni di Indentazione

Queste opzioni controllano come il codice viene indentato automaticamente.

: `vim.o.expandtab = true`

: Converte i caratteri di tabulazione in spazi, aiutando a mantenere una indentazione coerente e a evitare problemi di compatibilità tra diversi editor di testo.

: `vim.o.tabstop = 4`

: Definisce il numero di spazi rappresentati da un carattere di tabulazione (quando expandtab è disabilitato). Una larghezza di tab di 4 spazi è una convenzione comune per il codice Markdown.

: `vim.o.softtabstop = 4`

: Imposta il numero di spazi che vengono inseriti quando si preme il tasto <Tab>. Questo valore dovrebbe corrispondere alla larghezza del tab per mantenere la coerenza nell'indentazione.

: `vim.o.shiftwidth = 4`

: Definisce la larghezza dell'indentazione utilizzata dai comandi di indentazione automatica. Anche in questo caso, una larghezza di 4 spazi è una scelta comune per mantenere la leggibilità del codice.

: `vim.o.smartindent = true`

: Abilita l'indentazione intelligente, che adatta automaticamente l'indentazione in base al contesto del codice. Ciò può aiutare a ridurre la quantità di lavoro manuale necessaria per mantenere un'indentazione corretta.

### Opzioni dell'Interfaccia Utente (UI)

Queste opzioni personalizzano l'aspetto e il comportamento dell'interfaccia utente di Neovim.

: `vim.o.termguicolors = true`

: Abilita l'uso di colori a 24 bit (true color) nel terminale. Richiede un terminale che supporti true color per funzionare correttamente.

: `vim.o.mouse = "a"`

: Abilita l'uso del mouse in tutti le modalità (normale, visuale, inserimento, ecc.). Ciò può rende più intuitivo l'uso di Neovim per gli utenti abituati agli editor di testo con interfaccia grafica.

: `vim.o.showmode = false`

: Nasconde l'indicatore di modalità (ad esempio, "INSERT", "VISUAL") dalla riga di stato. Questo aiuta a ridurre la quantità di informazioni visualizzate e a mantenere la concentrazione sul codice.

: `vim.o.laststatus = 3`

: Imposta la riga di stato in modo che sia sempre visibile globalmente. La riga di stato fornisce informazioni importanti sullo stato corrente di Neovim, come la modalità attiva e la posizione del cursore.

: `vim.o.number = true`

: Mostra i numeri di riga. I numeri di riga possono essere molto utili per navigare nel codice e per riferirsi a specifiche linee durante la discussione o la documentazione.

: `vim.o.signcolumn = "yes"`

: Visualizza la colonna dei segni, utilizzata per visualizzare indicatori come i punti di interruzione del debugger o gli errori di linting. La colonna dei segni può essere personalizzata per mostrare diversi tipi di informazioni.

: `vim.o.splitbelow = true`  
`vim.o.splitright = true`

: Configurano il comportamento dei split verticali e orizzontali. I split permettono di visualizzare più file o parti di file contemporaneamente, migliorando la produttività e la gestione del codice.

### Opzioni per persisted.nvim

Queste opzioni sono specifiche per il plugin *persisted.nvim*, che permette di salvare e ripristinare sessioni di Neovim.

: `vim.o.sessionoptions = "buffers,curdir,folds,globals,tabpages,winpos,winsize"`

: Definisce quali elementi della sessione devono essere salvati e ripristinati. In questo caso, vengono salvati i buffer aperti, la directory corrente, lo stato dei fold, le variabili globali, le tab pages, la posizione e le dimensioni delle finestre. Ciò consente di ripristinare esattamente lo stato di lavoro precedente quando si riavvia Neovim.

## Conclusioni

In sintesi, il file *options.lua* fornisce una configurazione completa e personalizzata per *Neovim*, coprendo aspetti che vanno dalle opzioni di base dell'editor alle impostazioni avanzate per l'indentazione, l'interfaccia utente e la gestione delle sessioni. Questa configurazione è pensata per migliorare la produttività e l'esperienza di utilizzo di Neovim, adattandolo alle esigenze specifiche degli sviluppatori e del progetto *Rockmarker*.

L'uso delle opzioni permette di configurare l'editor sia nell'aspetto che nel comportamento per adattarlo ad esigenze specifiche, esistono opzioni specifiche per ogni aspetto di Neovim che possono essere consultate sulla [pagina relativa](https://neovim.io/doc/user/options.html) della documentazione di Neovim.
