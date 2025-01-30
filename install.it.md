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
# Rocksmarker

## IDE Neovim per il codice Markdown

Un progetto IDE **sperimentale** per la scrittura di documentazione in codice Markdown; il progetto utilizza il nuovo gestore di plugin [rocks.nvim](https://github.com/nvim-neorocks/rocks.nvim).  
**Rocks.nvim** si differenzia dai precedenti gestori di plugin per la sua filosofia di fondo, ovvero che la scrittura della configurazione di base (*dipendenze*, *opzioni di base* ...) è compito dello sviluppatore e non dell'utente. Questo permette all'utente finale di avere un'esperienza iniziale più semplice, mentre la personalizzazione avanzata dei plugin tramite file di configurazione rimane sempre possibile.  
Gli sviluppatori hanno anche creato un'infrastruttura per la distribuzione dei plugin, che non vengono scaricati dai rispettivi progetti, ma dal portale [luarocks](https://luarocks.org/modules/neorocks); in questo modo i pacchetti vengono prima testati e poi rilasciati. Di conseguenza, il rilascio di nuove versioni avviene solitamente in ritardo rispetto alle modifiche apportate ai progetti.

## Scopo del progetto

Fornire un editor il più completo possibile per la scrittura di documentazione Markdown per Rocky Linux; a tal fine gli obiettivi prefissati sono:

- Impostazione automatica delle opzioni di Neovim per i file Markdown
- Evidenziare i tag Markdown nel buffer
- Offrire una modalità zen per la modifica dei documenti
- Fornisce snippet personalizzati per la scrittura dei tag [mkdocs-material](https://squidfunk.github.io/mkdocs-material/), ma anche snippet per i tag Markdown standard.

## Prerequisiti per Neovim, Lua e Rocksmarker

> [!IMPORTANT]
> Per installare il pacchetto *ninja-build*, è necessario abilitare il [repository CRB](https://wiki.rockylinux.org/rocky/repo/#notes-on-crb) (CodeReady Linux Builder). Il repository fornisce strumenti comuni per lo sviluppo del codice e in Rocky Linux può essere abilitato con i seguenti comandi:

```bash
sudo dnf install -y epel-release yum-utils
sudo dnf config-manager --set-enabled crb
```

Per completare questo progetto è necessario installare alcuni pacchetti:

```bash
dnf install npm ncurses readline-devel icu ninja-build cmake gcc make unzip gettext curl glibc-gconv-extra tar git
```

## Installazione di Neovim

Per Rocksmarker, si consiglia di utilizzare la versione sorgente di Neovim. È possibile seguire le istruzioni contenute nella sezione "Avvio rapido" del [sito di Neovim] (<https://github.com/neovim/neovim/blob/master/BUILD.md>).

La compilazione da sorgente non presenta particolari problemi e, se si soddisfano i requisiti di cui sopra, si ottiene la seguente sequenza di comandi:

```bash
git clone https://github.com/neovim/neovim
cd neovim/
git checkout stable
make CMAKE_BUILD_TYPE=RelWithDebInfo
sudo make install
```

> [!NOTE]
> L'uso del comando *git checkout stable* per posizionare *git* nel ramo stabile prima della clonazione, assicura l'uso della versione stabile 0.10.2 (raccomandata). Se viene omesso, la compilazione verrà eseguita con il ramo di sviluppo 0.11.

## Installazione di Lua 5.1

Il gestore di plugin scelto per configurare i plugin aggiuntivi di Neovim (**rocks.nvim**), richiede l'installazione della versione *Lua 5.1* e che sia la versione predefinita, per funzionare correttamente. È inoltre necessario collegare i file *headers* della versione 5.1 a quelli presenti nel sistema.  
One of the features of Lua, resulting from its development as an “embedded” language, is that it does not require any external dependencies. As a result, the various versions can coexist on the same system without conflicting. For example, a desktop installation of Rocky Linux 9 provides version 5.4.4.  
You can download the 5.1 version from the [official Lua site](https://www.lua.org/download.html) with the command:

```bash
curl -O https://www.lua.org/ftp/lua-5.1.5.tar.gz
```

Una volta scaricato, scompattatelo e cambiatelo nella cartella `lua-5.1.5`:

```bash
tar xzf lua-5.1.5.tar.gz
cd lua-5.1.5
```

Anche se è possibile eseguire l'installazione a livello di utente, in questa configurazione si sceglie quella standard (in `/usr/local/`), che consente una configurazione successiva più pulita dei file *headers*.
You will need the *readline-devel* add-on package in the official repositories from Rocky Linux. It is in the prerequisites above.

> [!NOTE]
> Il pacchetto assume questo nome nelle distribuzioni derivate da RHEL, ma in altre si identifica in modo diverso. Debian, ad esempio, lo identifica come *libreadline-dev*.

Compilare e installare la versione con:

```bash
make linux test
sudo make install
```

L'installazione copierà i file nelle seguenti posizioni:

- **lua** **luac** -> `/usr/local/bin`
- **lua.h** **luaconf.h** **lualib.h** **lauxlib.h** **lua.hpp** -> `/usr/local/include`
- **liblua.a** -> `/usr/local/lib`

Per la sua rimozione è sufficiente eliminare i file elencati sopra.

### Impostazione della versione

Se il sistema in uso è una versione desktop, molto probabilmente è già installata una versione di Lua predefinita. È improbabile che questa versione corrisponda a quella richiesta, quindi è necessario impostarla per puntare alla versione corretta.  
Utilizzando un *alias* in *.bashrc* è possibile indicare al sistema la versione desiderata. La stringa da aggiungere è la seguente:

```bash
alias lua=/usr/local/bin/lua
```

Eseguire il *source* con:

```bash
. ~/.bashrc
```

Verificare il cambio di versione con:

```bash
lua -v
Lua 5.1.5  Copyright (C) 1994-2012 Lua.org, PUC-Rio
```

### Aggiungere file di intestazione

L'installazione della versione richiesta non è sufficiente perché la configurazione funzioni correttamente. È necessario collegare la libreria richiesta, *lua.h*, presente in `/usr/local/include/` nel percorso di ricerca del file *header* (`/usr/include/`), altrimenti non viene trovata da *luarocks* che si occupa dell'installazione del plugin *rocks.nvim*.
To accomplish this, you use one of the standard *Luarocks* search paths (`/usr/include/lua/<version_number>`). The *lua* folder is not present in a Rocky Linux system, and you will need to create it. Do this with:

```bash
cd /usr/include/
sudo mkdir lua && cd lua
sudo ln -s /usr/local/include/ 5.1
```

## Scaricare la configurazione

La configurazione, sebbene ancora in fase di sviluppo, può essere usata quotidianamente per scrivere e modificare documentazione scritta in Markdown, quindi può essere installata come configurazione predefinita nel percorso `.config/nvim`.  
Per gli utenti che hanno già una configurazione di Neovim presente sul sistema, esiste la possibilità di usare *rocksmarker* come editor secondario, consentendo così di continuare a usare la configurazione esistente per sviluppare i propri progetti.  
Questo metodo vi permette anche di provare *rocksmarker*, in modo del tutto indipendente, per valutare se può essere uno strumento utile per il vostro lavoro quotidiano.

### Editore principale

Per installare la configurazione nella posizione predefinita di Neovim, clonare il repository GitHub nella cartella delle configurazioni `~/.config/` con il comando:

```bash
git clone https://github.com/ambaradan/rocksmarker.git ~/.config/nvim
```

Una volta terminato, è sufficiente richiamare il comando standard di Neovim per avviare l'installazione:

```bash
nvim
```

### Editore secondario

Per testare o utilizzare la configurazione come configurazione secondaria, utilizzare la variabile di Neovim *NVIM_APPNAME*; l'uso di questa variabile consente a Neovim di passare un nome arbitrario che viene utilizzato per la ricerca dei file di configurazione in `~/.config/` e per la successiva creazione della cartella dei file condivisi in `~/.local/share/` e della cache in `~/.cache/`.  
Per poi impostare *segnapunti* come tipo di editor secondario:

```bash
git clone https://github.com/ambaradan/rocksmarker.git ~/.config/rocksmarker/
```

Una volta completata l'operazione di clonazione, avviare Neovim con il seguente comando per iniziare l'installazione:

```bash
NVIM_APPNAME=rocksmarker nvim
```

> [!IMPORTANT]
> Se si sceglie questo metodo, tutti i successivi avvii della configurazione devono essere effettuati con il comando descritto sopra, altrimenti Neovim si avvierà utilizzando la cartella predefinita `~/.config/nvim`. Per evitare di digitare ogni volta l'intero comando, si raccomanda la creazione di un *alias*.

## Installazione della configurazione

All'avvio di Neovim, con uno dei due metodi sopra descritti, inizierà il processo di installazione gestito da uno script *bootstrap* che ha verificato la mancanza del plugin *rocks.nvim* e ha proceduto alla sua installazione.  
Il primo passo dell'installazione consiste nella semplice installazione del gestore di plugin *rocks.nvim* al termine del quale, se tutto ha funzionato correttamente, vi verrà chiesto di premere INVIO per continuare.  
Il secondo passo è la sincronizzazione di tutti i plugin configurati; la sincronizzazione installa i plugin nella cartella dei file condivisi nel percorso `.local/share/nvim/rocks/lib/luarocks/rocks-5.1/`.

```text
:Rocks sync
```

Una volta terminata l'installazione dei plugin chiudere l'editor e riaprirlo per dare a Neovim la possibilità di caricare le nuove configurazioni, al secondo avvio anche i plugin *mason-lspconfig* e *mason-tool-installer* installati durante la sincronizzazione si occuperanno, in modo del tutto automatico, di installare tutti i server linguistici (LSP) necessari al corretto funzionamento dell'editor, terminata l'installazione dei server linguistici l'editor è pronto per essere utilizzato, buono sviluppo.

Per una panoramica grafica dell'editor, visitare la pagina [screenshots](./screenshots.md)
