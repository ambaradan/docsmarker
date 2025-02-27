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

Nel complesso, l'interfaccia utente di Neovim è altamente personalizzabile e flessibile, consentendo agli utenti di creare un ambiente di editing personalizzato. Con la giusta combinazione di colori, linee di stato, font, legami con i tasti e plugin, è possibile creare un'esperienza di editing produttiva ed efficiente in Neovim.

#### bamboo.nvim

**Bamboo.nvim** fornisce un tema verde scuro per Neovim scritto in Lua con l'evidenziazione della sintassi di Tree-sitter e l'evidenziazione semantica di LSP.  
Il tema usa il blu e il viola con parsimonia per ridurre l'affaticamento degli occhi. Il rosso, il giallo e il verde sono invece più usati per lo stesso motivo.  
I commenti sono colorati in modo specifico per essere leggibili e avere un buon contrasto con il resto del testo e dello sfondo. Sono gestiti e colorati in modo appropriato molti token dell'evidenziazione semantica e sono inoltre supportati i plugin più comuni con i colori impostati in maniera appropriata.

#### feline.nvim

Feline è una statusline minimale e completamente personalizzabile, fornisce una struttura su cui costruire facilmente la propria statusline, potendone modificare ogni minimo dettaglio a proprio piacimento.

Caratteristiche

- Facilità d'uso.
- Completa personalizzazione di ogni componente.
- Fornitori incorporati per cose come la modalità vi, le informazioni sui file, la dimensione dei file, la posizione del cursore, la diagnostica (usando l'LSP incorporato di Neovim), i rami e le differenze di git (usando gitsigns.nvim), ecc.
- Minimalista, fornisce solo il minimo indispensabile e consente all'utente di costruire i propri componenti con estrema facilità.

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

La tabella vim_mode definisce un componente per il plugin Feline, che è uno statusline personalizzabile per Neovim.
La tabella provider specifica il provider di questo componente, responsabile della generazione del contenuto da visualizzare. In questo caso, il provider è “vi_mode”, che visualizza la modalità Vim corrente.
La tabella opts all'interno della tabella provider imposta le opzioni per il provider, come ad esempio se mostrare il nome della modalità e come allineare il nome della modalità all'interno del componente.
La funzione hl definisce l'evidenziazione del componente. Utilizza il modulo require(“feline.providers.vi_mode”) per ottenere il nome e il colore dell'evidenziazione per la modalità Vim corrente, quindi imposta il colore di sfondo, il colore di primo piano e lo stile del testo.

I campi left_sep e right_sep definiscono i separatori da usare rispettivamente sui lati sinistro e destro del componente.

Questa configurazione visualizza la modalità Vim corrente nella riga di stato, con il nome della modalità centrato e il componente evidenziato in base alla modalità corrente.

Il codice Lua fornito imposta una tabella di configurazione c con una tabella nidificata vim_mode. Questa tabella vim_mode contiene informazioni su un componente in una configurazione Feline.
