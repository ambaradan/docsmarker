---
title: Install
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!--vale off-->

## Introduzione

Rocksmarker è un progetto IDE sperimentale per la scrittura di documentazione in Markdown che sfrutta il nuovo gestore di plugin **rocks.nvim** per Neovim.  
Per sfruttarne appieno le potenzialità e garantire un'installazione corretta, è fondamentale preparare adeguatamente l'ambiente Linux sottostante. Questo capitolo illustra i passaggi necessari, dalla configurazione dei repository all'installazione di Neovim e di una versione specifica di Lua, essenziale per il corretto funzionamento di Rocksmarker.

### Prerequisiti

- Una distribuzione Linux installata e configurata in versione desktop. In questa guida è utilizzata Rocky Linux e di conseguenza dovrebbe funzionare correttamente su tutte le derivate RHEL (Red Hat Enterprise Linux). Per l'installazione su altre distribuzioni fare riferimento alle relative documentazioni.
- Padronanza nell'esecuzione di comandi da terminale.
- Possibilità di eseguire alcuni comandi come utente root o con i permessi di amministratore.

### Prerequisiti per Neovim, Lua e Rocksmarker

Il progetto necessita di alcune dipendenze specifiche per la sua corretta esecuzione, in particolare è necessaria la versione ==Lua 5.1==, non sono supportate le versioni fornite da Rocky Linux.  
La versione 5.1 garantisce il corretto funzionamento di *rocks.nvim* che si occupa della gestione dei plugin garantendo inoltre anche la piena compatibilità con la versione utilizzata da Neovim.

Da Neovim docs:

> The Lua 5.1 script engine is builtin and always available.

Sono inoltre necessari alcuni pacchetti aggiuntivi, come il pacchetto *ninja-build*, forniti dal [repository CRB](https://wiki.rockylinux.org/rocky/repo/#notes-on-crb) (CodeReady Linux Builder). Il repository fornisce strumenti comuni per lo sviluppo del codice e in Rocky Linux può essere abilitato con i seguenti comandi:

```bash
sudo dnf install -y epel-release yum-utils
sudo dnf config-manager --set-enabled crb
```

Una volta impostate le fonti è necessario installare alcuni pacchetti aggiuntivi richiesti sia per la costruzione di *Neovim* che per l'installazione della versione Lua richiesta, per installarli su una distribuzione Rocky Linux digitare:

```bash
dnf install npm ncurses readline-devel icu ninja-build cmake gcc make unzip gettext curl glibc-gconv-extra tar git
```

Terminata l'installazione si può passare alla costruzione della struttura che prevede, come primo passo l'installazione dell'editor *Neovim*.

## Installazione di Neovim

*Neovim* è un editor di testo moderno, potente e altamente personalizzabile che offre un'esperienza familiare agli utenti di Vim.  
Pur mantenendo la filosofia di Vim introduce al contempo miglioramenti e funzionalità con l'obiettivo di fornire una esperienza utente similare ma su una base di codice più moderna.  
Tra i suoi miglioramenti troviamo:

- **Stabilità migliorata**: La base di codice è stata rifattorizzata per migliorare la stabilità e le prestazioni.
- **Sistema di plugin migliorato**: Neovim consente una gestione semplificata dei plugin rendendo più facile estenderne le sue funzionalità.
- **Caratteristiche moderne**: Supporta funzioni come le operazioni asincrone e una migliore integrazione con altre applicazioni.

!!! note "Versione compilata"

    Per una corretta esecuzione di Rocksmarker è necessaria una versione ==0.11.0== o superiore di Neovim, per questo si consiglia l'utilizzo della versione compilata. La versione fornita da *EPEL* risulta datata (0.8.0) e non è compatibile con *rocks.nvim* e i plugin utilizzati.  
    Per ulteriori informazioni è possibile consultare le istruzioni contenute nella sezione [Avvio rapido](https://github.com/neovim/neovim/blob/master/BUILD.md) del sito di Neovim.

La compilazione di Neovim da sorgente non presenta particolari problemi e, se si soddisfano i requisiti di cui sopra risulta agevole da svolgere.

### Scaricare i sorgenti

Il progetto viene sviluppato su *GitHub* e per recuperare i sorgenti è necessario fare il clone del repository localmente. Portarsi quindi nel percorso scelto del proprio file system e scaricarli con:

```bash
git clone https://github.com/neovim/neovim
```

Il comando crea una cartella `neovim` e scarica tutti i file al suo interno. Terminato il clone portarsi nella cartella appena creata e passare alla versione ==stabile== con i comandi:

```bash
cd neovim/
git checkout stable
```

!!! note ""

    L'uso del comando ==git checkout stable== per posizionare *git* nel ramo stabile prima della compilazione assicura l'uso della versione stabile (raccomandata). Se viene omesso, la compilazione verrà eseguita con il ramo di sviluppo (al momento la 0.12).

### Compilazione e installazione

Per la sua compilazione è sufficiente eseguire un solo comando:

```bash
make CMAKE_BUILD_TYPE=RelWithDebInfo
```

Il comando richiama *make* che si occupa dell'intero processo al quale viene passata la flag **CMAKE_BUILD_TYPE** che specifica il tipo di compilazione.  
In particolare la compilazione **RelwithDebInfo** produce codice completamente ottimizzato, ma costruisce anche il database del programma e inserisce informazioni sulle linee di debug utili per eventuali errori dell'editor.

Terminata la compilazione è possibile verificare la corretta esecuzione della nuova costruzione anche senza installarla. Neovim non necessita di componenti esterni per funzionare e conseguentemente è sufficiente richiamare l'eseguibile appena compilato per provare a fondo la nuova versione dell'editor prima di sostituire la versione utilizzata.

```bash
./neovim/build/bin/nvim
```

La sua installazione, una volta verificato il corretto funzionamento, viene eseguita sempre da *make*. I file, come per l'installazione di Lua, vengono copiati in `/usr/local` e anche in questo caso il comando deve essere eseguito come utente *root* o con i permessi di amministratore.

```bash
sudo make install
```

L'installazione rende disponibile nel sistema il nuovo comando ==nvim== per l'apertura dell'editor nel terminale che può essere usato anche per verificare la versione installata:

```bash
nvim --version
NVIM v0.10.4
Build type: RelWithDebInfo
LuaJIT 2.1.1713484068
Run "nvim -V1 -v" for more info
```

!!! tip "Rimozione di Neovim"

    Per la sua rimozione viene fornito un obiettivo CMake che rimuove tutti i file installati da ==make install==

    ```bash
    sudo cmake --build build/ --target uninstall
    ```

    È opportuno quindi conservare la cartella con i sorgenti per una eventuale rimozione.

Ora che l'editor è pronto all'uso si può passare all'installazione della versione di *Lua* corrispondente a quella fornita da *Neovim*.

## Installazione di Lua 5.1
  
Il gestore di plugin scelto per configurare i plugin aggiuntivi di Neovim (**rocks.nvim**), richiede l'installazione della versione *Lua 5.1* per funzionare correttamente e che la stessa sia la versione predefinita per quello spazio utente. Richiede inoltre il collegamento dei file *headers* della versione 5.1 a quelli presenti nel sistema.  

!!! note "Coesistenza di più versioni"

    Una delle caratteristiche di Lua, derivante dal suo sviluppo come linguaggio “embedded”, è che non richiede dipendenze esterne. Di conseguenza, le varie versioni possono coesistere sullo stesso sistema senza entrare in conflitto.  
    Ad esempio, un'installazione desktop di Rocky Linux 9 fornisce la versione 5.4.4 che viene utilizzata dai vari applicativi che la richiedono anche se la versione predefinita è la 5.1.

### Scaricare la versione richiesta

La versione 5.1 è disponibile sul sito ufficiale Lua nella pagina di [download](https://www.lua.org/download.html), l'ultima versione aggiornata è la ==5.1.5== che può essere scaricata con il comando:

```bash
curl -O https://www.lua.org/ftp/lua-5.1.5.tar.gz
```

Una volta scaricata verificare l'integrità del pacchetto scaricato con l'utilità *sha256* confrontando il risultato del seguente comando con il *checksum* fornito dal sito di download:

```bash
sha256sum lua-5.1.5.tar.gz 
2640fc56a795f29d28ef15e13c34a47e223960b0240e8cb0a82d9b0738695333  lua-5.1.5.tar.gz
```

Verificata l'integrità dello scaricamento scompattare l'archivio compresso e posizionarsi al suo interno:

```bash
tar xzf lua-5.1.5.tar.gz
cd lua-5.1.5
```

### Compilazione ed installazione

La sua installazione è una installazione di sistema (in `/usr/local/`), questo per consentire una successiva configurazione semplificata dei file *headers*, sono necessari di conseguenza in questa fase i permessi di amministratore.

Per una corretta compilazione è necessario il pacchetto aggiuntivo *readline-devel* disponibile nei repository ufficiali di Rocky Linux. Il pacchetto dovrebbe essere già disponibile se sono stati installati i pacchetti indicati nei prerequisiti di cui sopra.

!!! note ""

    Il pacchetto assume questo nome nelle distribuzioni derivate da RHEL, ma in altre si identifica in modo diverso. Debian, ad esempio, lo identifica come *libreadline-dev*.

Compilare la versione con:

```bash
make linux test
```

Il comando esegue un controllo dei requisiti necessari e procede alla compilazione dei binari. Se tutto termina senza errori viene stampato a terminale il messaggio seguente:

> Hello world, from Lua 5.1!

Per la sua installazione si utilizza l'utilità di sistema *make*, il comando deve essere eseguito come utente *root* o con i privilegi di amministratore:

```bash
sudo make install
```

L'installazione copia i file della versione 5.1.5 nel percorso `/usr/local/` mantenendoli così separati da quelli della versione di sistema installati per impostazione predefinita in `/usr/`.  
Vengono installati i seguenti file:

- **lua** - **luac** -> `/usr/local/bin/`
- **lua.h** - **luaconf.h** - **lualib.h** - **lauxlib.h** - **lua.hpp** -> `/usr/local/include/`
- **liblua.a** -> `/usr/local/lib/`

!!! tip ""

    La compilazione non fornisce una utilità per la rimozione dell'installazione. La rimozione può essere effettuata manualmente eliminando i file elencati sopra.

### Impostazione della versione

Se il sistema in uso è una versione desktop, molto probabilmente è già installata una versione di Lua predefinita. È però improbabile che questa versione corrisponda a quella richiesta, quindi è necessario impostare la versione predefinita per puntare alla versione corretta.

!!! note "Versione preinstallata"

    La versione installata sul sistema sarà sicuramente successiva alla 5.1 ma a riguardo bisogna evidenziare che le versioni successive alla 5.1 non sono considerate stabili in quanto non compatibili fra di loro.  
    Per verificare la versione installata digitare:

    ```bash
    lua -v
    Lua 5.4.4  Copyright (C) 1994-2022 Lua.org, PUC-Rio
    ```

L'intero progetto si basa sulla versione stabile di Lua utilizzata sia da Neovim che da rocks.nvim ed è indispensabile che la versione predefinita per il profilo utente sia la 5.1.  
Per soddisfare questo requisito aggiungere un *alias* in *.bashrc* per indicare al sistema la versione da utilizzare come predefinita per quel spazio utente.
Aprire il proprio `.bashrc` in un editor ed aggiungere la stringa seguente:

```bash
alias lua=/usr/local/bin/lua
```

Salvare il file ed eseguire il *source* per rileggere la configurazione con:

```bash
. ~/.bashrc
```

Una volta riletto il file verificare il cambio di versione con:

```bash
lua -v
Lua 5.1.5  Copyright (C) 1994-2012 Lua.org, PUC-Rio
```

Ora tutte le volte che verrà richiesto l'eseguibile sarà utilizzata la versione richiesta al posto di quella di sistema.

### Aggiungere i file di intestazione

L'installazione della versione richiesta non è sufficiente perché la configurazione funzioni correttamente. Il gestore dei plugin *rocks.nvim* necessita dei file *headers* di Lua per compilare la sua versione di *luarocks*.  
È necessario quindi collegare la libreria richiesta, **lua.h**, presente in `/usr/local/include/` nel percorso di ricerca dei file *headers* (`/usr/include/`).

!!! warning ""

    La mancanza di questo file non permette alla script di inizializzazione di compilare *luarocks* che termina con un errore interrompendo l'intero processo.

Per il suo collegamento si utilizza uno dei percorsi di ricerca standard di *luarocks* `/usr/include/lua/<number_version>` collegato alla cartella con i file *headers* della versione 5.1 in `/usr/local/include/`.

Per la sua realizzazione è necessario anche in questo caso operare come utente root o con i permessi di amministratore. Portarsi quindi nella cartella `include`:

```bash
cd /usr/include/
```

Creare la cartella `lua` e posizionarsi al suo interno, la cartella `/usr/include/lua/` non è presente in un sistema Rocky Linux ed è necessario quindi crearla.

```bash
sudo mkdir lua && cd lua
```

Creare quindi il collegamento simbolico alla cartella `/usr/local/include/`.

```bash
sudo ln -s /usr/local/include/ 5.1
```

Il comando collega la cartella dove sono stati copiati i file *headers* durante l'installazione di Lua 5.1 ad una cartella denominata `5.1` creata dal comando stesso.  
La denominazione della cartella è arbitraria ma scegliendo di usare il numero della versione consente un richiamo mnemonico al suo contenuto e cosa più importante soddisfa i requisiti per la ricerca dei percorsi di *luarocks*.

Per la sua verifica basta elencare la cartella `5.1` e dovrebbero comparire i file contenuti in `/usr/local/include/`:

```bash
ls -l /usr/include/lua/5.1/
total 52
-rw-r--r--. 1 root root  5777 27 dic  2007 lauxlib.h
-rw-r--r--. 1 root root 22299 11 feb  2008 luaconf.h
-rw-r--r--. 1 root root 11688 13 gen  2012 lua.h
-rw-r--r--. 1 root root   191 23 dic  2004 lua.hpp
-rw-r--r--. 1 root root  1026 27 dic  2007 lualib.h
```

Con questo ultimo passo l'ambiente per l'installazione è completo, sono soddisfatti tutti i requisiti necessari e si può passare all'installazione dell'editor.

## Scaricare la configurazione

La configurazione, sebbene ancora in fase di sviluppo, può essere usata quotidianamente per scrivere e modificare documentazione scritta in Markdown, quindi può essere installata come configurazione predefinita nel percorso `.config/nvim`.  
Per gli utenti che hanno già una configurazione di Neovim presente sul sistema, esiste la possibilità di usare *Rocksmarker* come editor secondario, consentendo così di continuare a usare la configurazione esistente per sviluppare i propri progetti.  
Questo metodo vi permette anche di provare *Rocksmarker*, in modo del tutto indipendente, per valutare se può essere uno strumento utile per il vostro lavoro quotidiano.

### Editor principale

Per installare la configurazione nella posizione predefinita di Neovim, clonare il repository GitHub nella cartella delle configurazioni `~/.config/` con il comando:

```bash
git clone https://github.com/ambaradan/rocksmarker.git ~/.config/nvim
```

Una volta terminato, è sufficiente richiamare il comando standard di Neovim per avviare l'installazione:

```bash
nvim
```

### Editor secondario

Per testare o utilizzare la configurazione come configurazione secondaria, utilizzare la variabile di Neovim *NVIM_APPNAME*; l'uso di questa variabile consente a Neovim di passare un nome arbitrario che viene utilizzato per la ricerca dei file di configurazione in `~/.config/` e per la successiva creazione della cartella dei file condivisi in `~/.local/share/` e della cache in `~/.cache/`.  
Per impostare *Rocksmarker* come editor secondario:

```bash
git clone https://github.com/ambaradan/rocksmarker.git ~/.config/rocksmarker/
```

Una volta completato il clone avviare Neovim con il seguente comando per iniziare l'installazione:

```bash
NVIM_APPNAME=rocksmarker nvim
```

!!! warning "Avvii successivi"

    Se si sceglie questo metodo, tutti i successivi avvii della configurazione devono essere effettuati con il comando descritto sopra, altrimenti Neovim si avvierà utilizzando la cartella predefinita `~/.config/nvim`. Per evitare di digitare ogni volta l'intero comando, si consiglia la creazione di un *alias*.

    ```bash
    alias rocksmarker="NVIM_APPNAME=rocksmarker nvim"
    ```

## Installazione della configurazione

All'avvio di Neovim, con uno dei due metodi sopra descritti, inizierà il processo di installazione gestito da uno script *bootstrap* che verificata la mancanza del plugin *rocks.nvim* procederà alla sua installazione.  
Il processo installa prima *luarocks* richiesta come dipendenza dal plugin e successivamente il plugin stesso, al termine del quale se tutto ha funzionato correttamente, vi verrà chiesto di premere INVIO per continuare.

```text title="rocks.nvim bootstrap"
Downloading luarocks...
Configuring luarocks...                                                                                                            
Installing luarocks...                                                                                                             
Installing rocks.nvim...                                                                                                           
rocks.nvim installed successfully!                                                                                                 
Press ENTER or type command to continue 
```

Il secondo passo è la sincronizzazione di tutti i plugin configurati; la sincronizzazione installa i plugin nella cartella dei file condivisi nel percorso `.local/share/nvim/rocks/lib/luarocks/rocks-5.1/`.  
Rispondendo con ++"Y"++ inizierà il processo di installazione di tutti i plugin configurati.

```text
rocks.nvim: The following plugins were not found:                                                                                    
trouble.nvim, mason.nvim, mason-lspconfig.nvim, nvim-lspconfig, nvim-cmp,
mason-tool-installer.nvim, luasnip, markdown.nvim, markview.nvim,
markdown-table-mode.nvim, zen-mode.nvim, conform.nvim, nvim-cokeline,
gitsigns.nvim, bamboo.nvim, telescope.nvim, persisted.nvim, toggleterm.nvim,
neo-tree.nvim, neogit, nvim-spectre, indent-blankline.nvim, nvim-autopairs,
nvim-highlight-colors, yanky.nvim, cmp-buffer, cmp-nvim-lsp, cmp-path,
cmp_luasnip, diffview.nvim, friendly-snippets, rocks-treesitter.nvim,
telescope-cmdline.nvim, telescope-file-browser.nvim, telescope-frecency.nvim,
telescope-ui-select.nvim, tree-sitter-bash, rainbow-delimiters.nvim,
tree-sitter-cli, rocks-config.nvim, tree-sitter-css, rocks-edit.nvim,
tree-sitter-git_config, tree-sitter-git_rebase, tree-sitter-gitattributes,
tree-sitter-gitcommit, tree-sitter-gitignore, tree-sitter-html, tree-sitter-ini,
tree-sitter-json, tree-sitter-jsonc, tree-sitter-lua, tree-sitter-luadoc,
tree-sitter-markdown, tree-sitter-markdown_inline, tree-sitter-toml,
tree-sitter-vim, tree-sitter-vimdoc, tree-sitter-yaml, which-key.nvim,
fidget.nvim, feline.nvim, nvim-lint, rocks-lazy.nvim, rocks-git.nvim,
nvim-web-devicons.                  
                                                                                                                                     
Run 'Rocks sync'?                                                                                                                    
[Y]es, (N)o:  
```

Una volta terminata l'installazione dei plugin chiudere l'editor e riaprirlo per dare a Neovim la possibilità di caricare le nuove configurazioni, al secondo avvio inoltre vengono installati dai plugin *mason-lspconfig* e *mason-tool-installer*, in modo del tutto automatico, i *server linguistici* (LSP), i *linter* e i *formattatori* necessari al corretto funzionamento dell'editor, terminata l'installazione dei server linguistici l'editor è pronto per essere utilizzato.

## Conclusioni

Rocksmarker è una configurazione personalizzata di Neovim e ne integra tutte le funzionalità, fornisce inoltre una serie di funzionalità aggiuntive per la gestione dei file, per i repository git, per la diagnostica e per molto altro.  
Per una panoramica delle scorciatoie che attivano le funzioni si consiglia di consultare il file `/lua/mappings.lua`.  
In alternativa con il tasto ++space++ si attiva il menù dei comandi dove sono elencati tutti i comandi disponibili. Le lettere contrassegnate con un **+** forniscono ulteriori selezioni, selezionando la lettera corrispondente si passa al menu contestuale e si ritorna al menu principale con ++back++ mentre con ++esc++ si chiude il menu senza eseguire alcun comando.
