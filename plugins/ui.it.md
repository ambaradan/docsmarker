---
title: User Interface
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---
<!--vale off-->
## Introduzione

Neovim include molti miglioramenti rispetto a Vim per quanto riguarda l'interfaccia utente e le prestazioni. Neovim presenta un'interfaccia utente moderna e personalizzabile, è stato progettato per essere più efficiente e veloce di Vim, con un'impronta di memoria ridotta e prestazioni migliori per i file di grandi dimensioni.

L'interfaccia utente (UI) di Neovim è altamente personalizzabile e consente di creare un ambiente di editing personalizzato che si adatta al proprio flusso di lavoro.

In Rocksmarker per personalizzare l'interfaccia utente sono stati modificati i seguenti aspetti:

### Rocksmarker UI

- **Colori**: Neovim supporta vari temi di colore che modificano il colore del testo, dello sfondo e di altri elementi dell'editor. Per la configurazione di *Rocksmarker* è stato utilizzato il tema bamboo.nvim.
- **Statusline**: La statusline visualizza informazioni quali il nome del file corrente, la posizione del cursore e la modalità. Neovim consente di personalizzare la statusline per visualizzare le informazioni desiderate nel formato preferito. Per questa funzionalità è stato scelto feline.nvim una statusline minimale ma altamente personalizzabile.
- **Caratteri**: Neovim consente di impostare il tipo e la dimensione dei caratteri dell'editor. È possibile scegliere tra una serie di font compatibili con il sistema operativo in uso.
- **Mappature dei tasti**: Neovim consente di mappare i tasti a comandi o funzioni personalizzate. Questa funzione consente di creare scorciatoie per i comandi utilizzati più di frequente o di eseguire operazioni complesse con la pressione di un solo tasto.

Nel complesso, l'interfaccia utente di Neovim è altamente personalizzabile e flessibile, consentendo agli utenti di creare un ambiente di editing personalizzato. Con la giusta combinazione di colori, statusline, font, mappature e plugin, è possibile creare un'esperienza di editing produttiva ed efficiente.

#### bamboo.nvim

**Bamboo.nvim** fornisce un tema verde scuro per Neovim scritto in Lua con l'evidenziazione della sintassi di Tree-sitter e l'evidenziazione semantica di LSP.Il tema usa il blu e il viola con parsimonia per ridurre l'affaticamento degli occhi. Il rosso, il giallo e il verde sono invece più usati per lo stesso motivo.  
I commenti sono colorati in modo specifico per essere leggibili e avere un buon contrasto con il resto del testo e dello sfondo. Sono gestiti e colorati in modo appropriato molti token dell'evidenziazione semantica e sono inoltre supportati i plugin più comuni con i colori impostati in maniera appropriata.

Per una ulteriore personalizzazione dell'aspetto è a disposizione la sezione relativa nel file `lua/plugins/ui.lua`, la sua struttura è la seguente:

```lua
require("bamboo").setup({
 -- Main options --
 style = "vulgaris",
 colors = {
    bright_orange = '#ff8800', -- Define a new color
    green = '#00ffaa', -- Redefine an existing color
 },
 highlights = {
  ["FloatBorder"] = { fg = "$grey" },
  ["IblIndent"] = { fg = "$bg1" },
  ["htmlH1"] = { fg = "$bg_blue" },
  ["htmlH2"] = { fg = "$green" },
  ....
  ....
 },
})
```

Il codice carica il plugin **bamboo** tramite la funzione *require('bamboo').setup*, imposta lo stile del tema e definisce due sezioni:

- **colors**: Questa sezione consente di definire nuovi colori o di ridefinire quelli esistenti. In questo caso, come esempio, viene definito un nuovo colore bright_orange e viene ridefinito il colore verde esistente.
- **highlights**: Questa sezione consente di definire l'evidenziazione della sintassi per vari costrutti linguistici e per le definizioni degli aspetti grafici di Neovim come bordi, sfondi e altro.

In questo modo si possono personalizzare i colori dell'interfaccia in modo flessibile ed estensibile.

!!! Tip "Ricavare il nome del token"

    Per ricavare i token semantici di *Neovim*, di *Tree-sitter* e dei vari *LSP* utilizzare il comando ==:Inspect==. La digitazione del comando, posizionando il cursore sulla parola o tag scelto, fornirà il nome del *token* (utilizzabile per la personalizzazione) e il suo colore attuale. Ricavato il nome del *token* è possibile ispezionare meglio le sue proprietà con il comando ==:Telescope highlights== inserendo nella ricerca il nome ottenuto con *Inspect*.

#### feline.nvim

Feline è una statusline minimale e completamente personalizzabile, fornisce una struttura modulare su cui costruire facilmente la propria statusline, consente di modificare ogni minimo dettaglio a proprio piacimento. Si distingue per la facilità d'uso e per la completa personalizzazione di ogni componente.  
Fornisce moduli incorporati per cose come la modalità vi, le informazioni sui file, la dimensione dei file, la posizione del cursore, la diagnostica (usando l'LSP incorporato di Neovim), i rami e le differenze di git (usando gitsigns.nvim), ecc.

Tutti i moduli sono contenuti in tabelle annidate in una tabella denominata **c**:

```lua
local c = {
 vim_mode = {

 },
....
....
 lsp_client_names = {

 },
...
...
```

Al suo interno troviamo, ad esempio, il modulo *vim_mode* che imposta i colori e l'aspetto dei vari stati vim (INSERT, VISUAL, ...):

```lua linenums="108"
 vim_mode = {
  provider = {
   name = "vi_mode",
   opts = {
    show_mode_name = true,
    padding = "center",
   },
  },
  hl = function()
   return {
    name = require("feline.providers.vi_mode").get_mode_highlight_name(),
    bg = require("feline.providers.vi_mode").get_mode_color(),
    fg = "bg3",
    style = "bold",
   }
  end,
  left_sep = "block",
  right_sep = "right_rounded",
 },
```

La relativa sintassi è la seguente:

La tabella **vim_mode** definisce i seguenti parametri del modulo. La tabella **provider** specifica il provider di questo componente, responsabile della generazione del contenuto da visualizzare. In questo caso, il provider è *vi_mode*, che visualizza la modalità Vim corrente.  
La tabella **opts** all'interno della tabella provider imposta le opzioni per il provider, come ad esempio se mostrare il nome della modalità e come allineare il nome della modalità all'interno del componente.  
La funzione **hl** definisce l'evidenziazione del componente. Utilizza la funzione *require(“feline.providers.vi_mode”)* per ottenere il nome e il colore dell'evidenziazione per la modalità Vim corrente, quindi imposta il colore di sfondo, il colore di primo piano e lo stile del testo.  
I campi **left_sep** e **right_sep** definiscono i separatori da usare rispettivamente sui lati sinistro e destro del componente.

Segue la definizione vera e propria della statusline che specifica il tipo e la posizione di ogni modulo.

```lua
local left = {
 c.vim_mode,
 c.gitBranch,
    ....
}

local middle = {
 c.lsp_client_names,
 c.diagnostic_errors,
    ....
}

local right = {
 -- c.file_type,
 c.file_encoding,
    ....
}

local components = {
 active = {
  left,
  ....
 },
 inactive = {
  left,
  ....
 },
}

```

Al termine abbiamo la funzione che unisce tutte le impostazioni precedenti:

```lua linenums="290"
feline.setup({
 components = components,
 theme = bamboo,
 vi_mode_colors = vi_mode_colors,
})
```

La funzione *feline.setup()* serve a configurare il plugin feline, la tabella dei **components** viene passata come primo argomento alla funzione *setup()*. Questa tabella contiene i vari componenti che saranno visualizzati nella statusline.  
Il parametro **theme** è impostato su *bamboo*, lo schema di colori predefinito per il plugin feline.  
Il parametro **vi_mode_colors** è impostato sulla tabella *vi_mode_colors*, che contiene le configurazioni dei colori per le diverse modalità di Vi che saranno usate nella statusline.

Nel complesso, questo codice imposta il plugin feline con i componenti, il tema e le configurazioni dei colori delle modalità Vi specificate, consentendo di ottenere una statusline personalizzata e molto informativa.

#### nvim-cokeline
