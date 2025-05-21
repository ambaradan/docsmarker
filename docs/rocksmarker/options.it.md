---
title: Neovim Options
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!--vale off-->
## Introduzione

Questo documento descrive il contenuto del file options.lua, utilizzato per configurare l'ambiente Neovim (o Vim) secondo le preferenze del progetto Rocksmarker. Il file è strutturato per impostare opzioni globali, opzioni dell'editor, impostazioni di indentazione e preferenze dell'interfaccia utente.

### Configurazione Globale

Questa sezione definisce variabili globali che influenzano il comportamento generale di Neovim.

* Leader Key: `vim.g.mapleader = vim.keycode("<Space>")`  
Imposta il tasto `<Space>` come leader key. Il leader key è un prefisso utilizzato per definire mappature personalizzate (scorciatoie da tastiera). Ciò significa che tutte le mappature personalizzate definite nel file di configurazione inizieranno con la pressione del tasto `<Space>`.
* Stile Markdown: `vim.g.markdown_recommended_style = 0`  
Disabilita lo stile raccomandato per il Markdown. Questo potrebbe essere fatto per evitare conflitti con lo stile preferito dal progetto Rocksmarker o per personalizzare ulteriormente la formattazione del Markdown.
* Disabilitazione Provider di Linguaggio:  
`vim.g.loaded_python3_provider = 0`  
`vim.g.loaded_ruby_provider = 0`  
`vim.g.loaded_perl_provider = 0`  
`vim.g.loaded_node_provider = 0`  
Disabilitano i provider di linguaggio per Python, Ruby, Perl e Node.js. Questo può essere fatto per ridurre l'overhead se questi linguaggi non sono necessari per il progetto, e per evitare avvisi di checkhealth relativi a dipendenze non soddisfatte per questi provider.

### Opzioni dell'Editor

Questa sezione configura il comportamento dell'editor di testo, influenzando la visualizzazione e l'interazione con i file.

* Folding del Codice:  
`vim.o.foldenable = true`  
Abilita il folding del codice, permettendo di comprimere e espandere sezioni di codice. Questo può aiutare a ridurre la quantità di codice visibile e a concentrarsi sulle parti più rilevanti.  
`vim.o.foldmethod = "marker"`  
Imposta il metodo di folding su "marker", il che significa che il folding è determinato da marcatori specifici nel codice. Questi marcatori possono essere inseriti manualmente dall'utente per definire le sezioni di codice da piegare.  
`vim.o.foldmarker = "{{{,}}}"`  
Definisce i marcatori utilizzati per delimitare le sezioni di codice da piegare. In questo caso, {{{ segna l'inizio e }}} la fine di una sezione pieghevole.

* Integrazione con la Clipboard: `vim.o.clipboard = "unnamedplus"`  
Configura l'integrazione con la clipboard di sistema (Linux). L'opzione "unnamedplus" permette di copiare e incollare direttamente con la clipboard del sistema operativo, senza la necessità di utilizzare comandi specifici di Vim.

* Riempimento dei Caratteri: `vim.o.fillchars = "eob: "`  
Personalizza i caratteri di riempimento. In questo caso, nasconde il carattere ~ (tilde) per le righe vuote alla fine del buffer, migliorando la leggibilità del codice.

* Timeout: `vim.o.timeoutlen = 400`  
Imposta il tempo di attesa (in millisecondi) dopo ogni tasto premuto prima di considerarlo parte di una sequenza di comandi. Questo può influenzare la velocità di esecuzione dei comandi e la sensibilità della tastiera.

* Cronologia degli Undo: `vim.o.undofile = true`  
Abilita il salvataggio automatico della cronologia degli undo, permettendo di annullare modifiche anche dopo aver chiuso e riaperto il file. Ciò può essere particolarmente utile per recuperare versioni precedenti del codice.

* Evidenziazione della Riga Corrente: `vim.o.cursorline = true`  
Evidenzia la riga corrente dove si trova il cursore, aiutando a mantenere la concentrazione sulla posizione corrente all'interno del codice.

* Ricerca Case-Insensitive:  
`vim.o.ignorecase = true`  
Imposta la ricerca in modo da non considerare la differenza tra maiuscole e minuscole, semplificando la ricerca di parole chiave all'interno del codice.  
`vim.o.smartcase = true`  
Modifica il comportamento di ignorecase in modo che la ricerca diventi case-sensitive solo se si utilizzano maiuscole nel pattern di ricerca. Ciò offre un equilibrio tra facilità di ricerca e precisione.

### Impostazioni di Indentazione

Queste opzioni controllano come il codice viene indentato automaticamente.

    Conversione dei Tab:
        vim.o.expandtab = true
        Converte i caratteri di tabulazione in spazi, aiutando a mantenere una indentazione coerente e a evitare problemi di compatibilità tra diversi editor di testo.
    Larghezza del Tab:
        vim.o.tabstop = 4
        Definisce il numero di spazi rappresentati da un carattere di tabulazione (quando expandtab è disabilitato). Una larghezza di tab di 4 spazi è una convenzione comune in molti progetti di codifica.
    Larghezza del Tab Soft:
        vim.o.softtabstop = 4
        Imposta il numero di spazi che vengono inseriti quando si preme il tasto <Tab>. Questo valore dovrebbe corrispondere alla larghezza del tab per mantenere la coerenza nell'indentazione.
    Larghezza dell'Indentazione:
        vim.o.shiftwidth = 4
        Definisce la larghezza dell'indentazione utilizzata dai comandi di indentazione automatica. Anche in questo caso, una larghezza di 4 spazi è una scelta comune per mantenere la leggibilità del codice.
    Indentazione Intelligente:
        vim.o.smartindent = true
        Abilita l'indentazione intelligente, che adatta automaticamente l'indentazione in base al contesto del codice. Ciò può aiutare a ridurre la quantità di lavoro manuale necessaria per mantenere un'indentazione corretta.

Opzioni dell'Interfaccia Utente (UI)

Queste opzioni personalizzano l'aspetto e il comportamento dell'interfaccia utente di Neovim.

    True Color:
        vim.o.termguicolors = true
        Abilita l'uso di colori a 24 bit (true color) nel terminale. Richiede un terminale che supporti true color per funzionare correttamente.
    Uso del Mouse:
        vim.o.mouse = "a"
        Abilita l'uso del mouse in tutti i modi (normale, visuale, inserimento, ecc.). Ciò può rendere più intuitivo l'uso di Neovim per gli utenti abituati agli editor di testo con interfaccia grafica.
    Indicatore di Modalità:
        vim.o.showmode = false
        Nasconde l'indicatore di modalità (ad esempio, "INSERT", "VISUAL") dalla riga di stato. Questo può aiutare a ridurre la quantità di informazioni visualizzate e a mantenere la concentrazione sul codice.
    Riga di Stato:
        vim.o.laststatus = 3
        Imposta la riga di stato in modo che sia sempre visibile globalmente. La riga di stato fornisce informazioni importanti sullo stato corrente di Neovim, come la modalità attiva e la posizione del cursore.
    Numeri di Riga:
        vim.o.number = true
        Mostra i numeri di riga. I numeri di riga possono essere molto utili per navigare nel codice e per riferirsi a specifiche linee durante la discussione o la documentazione.
    Colonna dei Segni:
        vim.o.signcolumn = "yes"
        Visualizza la colonna dei segni, utilizzata per visualizzare indicatori come i punti di interruzione del debugger o gli errori di linting. La colonna dei segni può essere personalizzata per mostrare diversi tipi di informazioni.
    Split Verticale e Orizzontale:
        vim.o.splitbelow = true
        vim.o.splitright = true
        Configurano il comportamento dei split verticali e orizzontali. I split permettono di visualizzare più file o parti di file contemporaneamente, migliorando la produttività e la gestione del codice.

Opzioni per Persisted.nvim

Queste opzioni sono specifiche per il plugin persisted.nvim, che permette di salvare e ripristinare sessioni di Neovim.

    Opzioni di Sessione:
        vim.o.sessionoptions = "buffers,curdir,folds,globals,tabpages,winpos,winsize"
        Definisce quali elementi della sessione devono essere salvati e ripristinati. In questo caso, vengono salvati i buffer aperti, la directory corrente, lo stato dei fold, le variabili globali, le tab pages, la posizione e le dimensioni delle finestre. Ciò consente di ripristinare esattamente lo stato di lavoro precedente quando si riavvia Neovim.

In sintesi, il file options.lua fornisce una configurazione completa e personalizzata per Neovim, coprendo aspetti che vanno dalle opzioni di base dell'editor alle impostazioni avanzate per l'indentazione, l'interfaccia utente e la gestione delle sessioni. Questa configurazione è pensata per migliorare la produttività e l'esperienza di utilizzo di Neovim, adattandolo alle esigenze specifiche degli sviluppatori e del progetto Rockmarkder.

## Introduzione

Una installazione standard di Neovim si presenta già pronta all'uso ma con un aspetto un po' semplice e certamente non ottimizzata per le vostre preferenze specifiche. Il primo passo per la personalizzazione dell'installazione passa dalla configurazione delle opzioni.  
Le opzioni di Neovim sono uno strumento potente e flessibile che consentono di modificare numerosi aspetti dell'editor, dai più basilari come la visualizzazione dei numeri di riga nel buffer ad altre più avanzate come la sincronizzazione della clipboard del sistema o le impostazioni di formattazione.  
La loro flessibilità consente di applicarle a vari livelli, a livello globale (==vim.g== - *global*) per le impostazioni generali dell'editor, a livello di finestra (==vim.wo== - *window options*) per eventuali impostazioni di una area di lavoro e a livello di buffer (==vim.bo== - *buffer options*) per una personalizzazione più granulare.

Per sfruttare tutto il potenziale di Neovim la configurazione utilizza [ftplugin](https://neovim.io/doc/user/filetype.html#%3Afiletype-plugin-on) per la configurazione dei tipi di file, di conseguenza le opzioni specifiche si trovano nei rispettivi file della cartella `ftplugin`.  
Questo consente di adattare le impostazioni ad ogni tipo di file specifico snellendo il flusso di lavoro e permettendo così di lavorare in un ambiente di codifica più organizzato ed efficiente.

## Opzioni globali

Le impostazioni globali di Rocksmarker sono definite nel file `lua/options.lua` e sono tutte commentate per una maggiore comprensione della loro influenza sul comportamento di Neovim. Le principali sono quelle definite all'inizio del file e sono le seguenti:

```lua
-- Global configurations
vim.g.mapleader = vim.keycode("<Space>") -- leader key
vim.g.markdown_recommended_style = 0 -- disable Markdown recommended style
-- disable providers
-- remove warnings in ':checkhealth'
vim.g.loaded_python3_provider = 0
vim.g.loaded_ruby_provider = 0
vim.g.loaded_perl_provider = 0
vim.g.loaded_node_provider = 0
```

Queste impostazioni determinano la chiave da utilizzare come *leader key*. La *leader key* è la chiave della tastiera che richiama tutte le altre combinazioni di chiavi, impostata a ++space++.  

!!! Note ""

    In Rocksmarker la sua pressione richiama il menu fornito da *which-key.nvim* che permette una navigazione migliorata tra i comandi forniti e la descrizione degli stessi.

Segue poi la disabilitazione dello stile raccomandato per il codice markdown ==vim.g.markdown_recommended_style = 0== in quanto non compatibile con la documentazione Rocky Linux.  
Lo stile integrato influisce principalmente sulla formattazione del codice fornita dal *server linguistico*, funzione questa che in Rocksmarker è gestita dal plugin *conform.nvim*. Lo stile integrato ad esempio imposta la larghezza del documento a 80 caratteri e già questo non è compatibile con il markdown scritto per la documentazione.

Le successive opzioni non hanno alcun effetto sulla configurazione dell'editor ma sono funzionali al debug, permettono di eliminare gli errori visualizzati dal controllo ==:checkhealth== riferiti a questi linguaggi che non sono oggetto del progetto.

!!! Tip "Risoluzione dei problemi"

    Il comando ==checkhealth== insieme al comando ==messages== sono strumenti indispensabili per la risoluzione dei problemi di configurazione.

### Opzioni dell'editor

Le successive opzioni impostano il comportamento dell'editor nella gestione del buffer, sono impostati comportamenti come il metodo di ricerca, la gestione della clipboard e altri aspetti.

```lua
vim.o.clipboard = "unnamedplus" -- sync with the Linux clipboard
vim.o.mouse = "a" -- enable the use of the mouse - all modes
vim.o.timeoutlen = 400 -- how long wait after each keystroke before aborting it
vim.o.undofile = true -- automatically save undo history
vim.o.cursorline = true -- highlight the current line
vim.o.ignorecase = true -- to search case insensitively
vim.o.smartcase = true -- when the search pattern is typed
```

Di seguito sono definite le opzione di indentazione, queste sono impostate tutte a quattro spazi come richiesto per la formattazione di codice markdown.

```lua
-- Indenting
vim.o.expandtab = true -- Convert tabs to spaces
vim.o.tabstop = 4 -- Number of spaces a tab represents
vim.o.softtabstop = 4 -- how many spaces moves right when you press <Tab>
vim.o.shiftwidth = 4 -- provide proper indentation to the code
vim.o.smartindent = true -- increase/reduce the indent where appropriate
```

!!! Note ""

    L'opzione ==vim.o.smartindent== consente di formattare correttamente le liste markdown che altrimenti verrebbero indentate a quattro spazi.

### Opzioni dell'interfaccia

Queste opzioni influenzano l'aspetto dell'interfaccia dell'editor, integrano il buffer con le caratteristiche proprie di un editor di codice. Nascondono inoltre alcuni comportamenti di default non necessari ad un editor markdown.

```lua
-- UI options
vim.o.termguicolors = true -- use true color
vim.o.fillchars = "eob:*" -- hide tilde '~' for blank lines
vim.o.showmode = false -- show the mode you are on the last line
vim.o.laststatus = 3 -- to global display the status line
vim.o.number = true -- enable line numbers
vim.o.signcolumn = "yes" -- displaying the signs
```

!!! Note ""

    L'opzione ==vim.o.laststatus = 3== serve a visualizzare una sola *statusline* che in caso contrario verrebbe visualizzata per buffer e quindi moltiplicata per il numero di buffer aperti, comportamento più adatto al codice di programmazione e non necessario in un editor markdown.

Le successive opzioni impostano il comportamento predefinito in cui vengono divise le finestre, questa impostazione è pensata principalmente per la consultazione delle pagine di aiuto di Neovim, in questo modo il comando ==:help== posiziona il buffer a destra rendendo comoda la sua consultazione mentre si edita il codice.

```lua
vim.o.splitbelow = true -- create a vertical split and open
vim.o.splitright = true -- new file in the right-hand side of the split
```

Il file delle opzioni termina con la definizione dell'opzione ==vim.o.sessionoptions==, questa opzione indica a Neovim quali parti dell'editor debbano essere salvate nella sessione all'uscita. Queste informazioni vengono utilizzate dal plugin *persisted.nvim* per la gestione delle sessioni.

```lua
-- require by persisted.nvim - enables saving and restoring
vim.o.sessionoptions = "buffers,curdir,folds,globals,tabpages,winpos,winsize"
```

## Conclusioni

L'uso delle opzioni permette di configurare l'editor sia nell'aspetto che nel comportamento per adattarlo ad esigenze specifiche, esistono opzioni specifiche per ogni aspetto di Neovim che possono essere consultate sulla [pagina relativa](https://neovim.io/doc/user/options.html) della documentazione di Neovim.
