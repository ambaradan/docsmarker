---
title: Layout
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

<!--vale off-->

# Configurazione di Rocksmarker

## Introduzione

La cartella plugins è strutturata in modo da separare le configurazioni per aree tematiche, rendendo più facile la gestione e la comprensione della configurazione di Rocksmarker. Ogni file Lua all'interno della cartella è responsabile della configurazione di uno o più plugin correlati. Questa modularità permette di attivare, disattivare o modificare le impostazioni di specifici plugin senza influenzare il resto della configurazione.

Di seguito è riportata una breve descrizione dei file presenti nella cartella plugins e delle loro funzionalità:

: `ui.lua`

L'obiettivo principale di ui.lua è di fornire un'esperienza utente raffinata e personalizzata in Neovim. Per raggiungere questo obiettivo, il file configura diversi plugin chiave:

- Tema dei colori (min-theme): Definisce i colori utilizzati per la sintassi, l'interfaccia e gli elementi visivi di Neovim. Permette di scegliere tra temi chiari e scuri, impostare la trasparenza dello sfondo e personalizzare l'aspetto di commenti, parole chiave, funzioni, stringhe e variabili tramite l'uso di stili corsivi.
- Barra di stato (lualine.nvim): Configura la barra di stato inferiore di Neovim per visualizzare informazioni utili come la modalità corrente, il nome del file, il branch Git, le modifiche non committate, lo stato dell'LSP, i risultati diagnostici, il tipo di file, il progresso nel file e la posizione del cursore.
- Gestione dei buffer (bufferline.nvim): Gestisce la visualizzazione e l'interazione con i buffer aperti in Neovim. Permette di visualizzare i buffer come una riga di tab, mostrare icone, indicatori di modifica e diagnostica, e personalizzare l'aspetto dei separatori e degli elementi selezionati.
- Visualizzazione dei messaggi (fidget.nvim): Migliora la visualizzazione dei messaggi e delle notifiche di Neovim, fornendo un'interfaccia più pulita e informativa.
- Integrazione Git (gitsigns.nvim): Visualizza indicatori visivi nel gutter per mostrare le modifiche apportate al file corrente rispetto all'ultima versione nel repository Git. Permette di navigare tra le modifiche, effettuare lo stage e il reset delle modifiche, visualizzare il blame e confrontare le versioni.
- Menu Contestuali (which-key.nvim): Fornisce un menu contestuale interattivo che mostra le scorciatoie da tastiera disponibili in base al contesto corrente.

: `diagnostics.lua`

L'obiettivo principale di diagnostics.lua è di integrare e configurare strumenti che supportano la qualità e la consistenza del codice. Per raggiungere questo obiettivo, il file gestisce diversi aspetti chiave:

- Formattazione del codice (conform.nvim): Configura il plugin conform.nvim per formattare automaticamente il codice al salvataggio del file. Definisce i formatter da utilizzare per diversi tipi di file (Lua, CSS, HTML, Shell Script, Markdown, YAML) e le opzioni di formattazione.
- Linting del codice (nvim-lint): Configura il plugin nvim-lint per eseguire linter (strumenti di analisi statica del codice) sui file al salvataggio. Definisce i linter da utilizzare per diversi tipi di file (Markdown, YAML, Bash) e configura l'esecuzione automatica del linting.
- Visualizzazione dei diagnostici (trouble.nvim): Configura il plugin trouble.nvim per visualizzare i diagnostici (errori, avvisi, suggerimenti) generati dai linter e dai server linguistici in una finestra dedicata.

: `lsp.lua`

L'obiettivo principale di lsp.lua è di integrare e configurare l'LSP in Neovim, fornendo un'esperienza di sviluppo fluida ed efficiente. Per raggiungere questo obiettivo, il file gestisce diversi aspetti cruciali:* Configurazione dei server linguistici (lspconfig): Imposta i server linguistici per diversi linguaggi di programmazione, come Lua, HTML, CSS, JavaScript, Python, e molti altri. Ogni server linguistico fornisce funzionalità specifiche per il linguaggio che supporta.

- Mappature di tasti LSP: Definisce le scorciatoie da tastiera per accedere alle funzionalità LSP, come "Vai alla definizione", "Mostra documentazione al passaggio del mouse", "Rinomina simbolo", "Trova riferimenti", "Vai all'implementazione" e "Mostra definizioni di tipo".
- Completamento automatico del codice (nvim-cmp): Configura il plugin nvim-cmp per fornire suggerimenti di completamento automatico del codice basati sulle informazioni fornite dai server linguistici. Integra anche snippet di codice (luasnip) per accelerare la scrittura del codice.
- Gestione e installazione dei server LSP (mason e mason-lspconfig): Utilizza i plugin mason e mason-lspconfig per semplificare l'installazione e l'aggiornamento dei server linguistici. Definisce una lista di server da installare automaticamente.
- Funzionalità aggiuntive: Abilita funzionalità LSP avanzate, come l'evidenziazione dei simboli in uso nel documento e la gestione dei diagnostici (errori, avvisi, suggerimenti) forniti dai server linguistici.

: `utils.lua`

L'obiettivo principale di plugins/utils.lua è di fornire una suite di strumenti che semplificano e potenziano il workflow di sviluppo in Neovim. Per raggiungere questo obiettivo, il file configura diversi plugin chiave:

- Ricerca e navigazione (telescope.nvim): Configura il plugin telescope.nvim per fornire un'interfaccia di ricerca e navigazione potente e flessibile. Permette di cercare file, buffer, simboli, comandi, cronologia, commit Git e altro ancora.
- Gestione delle sessioni (persisted.nvim): Configura il plugin persisted.nvim per salvare e ripristinare le sessioni di Neovim. Permette di riprendere il lavoro da dove lo si era interrotto, riaprendo automaticamente i file e i buffer.
- Terminali a scomparsa (toggleterm.nvim): Configura il plugin toggleterm.nvim per creare e gestire terminali a scomparsa all'interno di Neovim. Permette di eseguire comandi e script senza dover uscire dall'editor.
- Gestione dei file (neo-tree.nvim): Configura il plugin neo-tree.nvim per fornire un albero di file visuale all'interno di Neovim. Permette di navigare tra i file e le directory, creare, rinominare, eliminare e spostare file.
- Gestione Git (neogit.nvim): Configura il plugin neogit.nvim per fornire un'interfaccia Git all'interno di Neovim. Permette di visualizzare lo stato del repository, effettuare commit, push, pull, branch e altro ancora.
- Ricerca e sostituzione avanzata (spectre.nvim): Configura il plugin spectre.nvim per fornire uno strumento di ricerca e sostituzione avanzata basato su espressioni regolari.
- Gestione degli appunti (yanky.nvim): Configura il plugin yanky.nvim per gestire la cronologia degli appunti di Neovim. Permette di accedere ai testi copiati in precedenza e di sincronizzare gli appunti con il sistema operativo.
- Guide di indentazione (indent-blankline.nvim): Configura il plugin indent-blankline.nvim per visualizzare guide di indentazione verticali per migliorare la leggibilità del codice.
- Colorazione delle parentesi (rainbow-delimiters): Configura il plugin rainbow-delimiters per colorare le parentesi in modo da facilitare la comprensione della struttura del codice.
- Completamento automatico delle coppie di caratteri (nvim-autopairs.nvim): Configura il plugin nvim-autopairs per inserire automaticamente le coppie di caratteri (parentesi, virgolette, ecc.).
- Evidenziazione dei colori (nvim-highlight-colors.nvim): Configura il plugin nvim-highlight-colors per evidenziare i codici colore (es. #RRGGBB) nel codice.

: `markdown.lua`

L'obiettivo principale di plugins/markdown.lua è di fornire un ambiente di editing Markdown efficiente e piacevole. Per raggiungere questo obiettivo, il file configura diversi plugin chiave:

- Formattazione e anteprima (markview.nvim): Configura il plugin markview.nvim per migliorare la formattazione visiva dei file Markdown, in particolare per quanto riguarda le intestazioni, le tabelle e i blocchi di codice. Permette di personalizzare l'aspetto delle intestazioni e delle tabelle con diversi stili.
- Funzionalità Markdown avanzate (markdown.nvim): Configura il plugin markdown.nvim per aggiungere funzionalità avanzate all'editing Markdown, come la possibilità di inserire automaticamente elementi di formattazione (grassetto, corsivo, codice) e di gestire i link.
- Gestione delle tabelle (markdown-table-mode.nvim): Configura il plugin markdown-table-mode.nvim per semplificare la creazione e la modifica di tabelle Markdown. Permette di inserire automaticamente i bordi delle tabelle e di formattarle in modo coerente.
- Modalità di scrittura senza distrazioni (zen-mode.nvim): Configura il plugin zen-mode.nvim per attivare una modalità di scrittura senza distrazioni, nascondendo elementi dell'interfaccia utente non essenziali e focalizzando l'attenzione sul testo.

## Conclusioni

Un Ambiente Neovim Personalizzato e Potenziato

Attraverso la configurazione dettagliata delineata in questa documentazione, la cartella plugins emerge come il cuore pulsante di un ambiente Neovim altamente personalizzato e ottimizzato. Ogni file, con la sua specifica area di competenza, contribuisce a creare un'esperienza di editing che si adatta perfettamente alle esigenze e alle preferenze individuali.

Riepilogo dei Punti Chiave:

- Modularità e Organizzazione: La struttura modulare della cartella plugins, con file dedicati a specifiche aree funzionali, semplifica la gestione e la manutenzione della configurazione di Neovim.
- Potenziamento dell'Interfaccia Utente: Il file ui.lua trasforma l'aspetto visivo di Neovim, offrendo un'interfaccia utente elegante, informativa e personalizzata.
- Integrazione Avanzata dell'LSP: Il file lsp.lua trasforma Neovim in un potente IDE, fornendo funzionalità avanzate di completamento automatico del codice, navigazione, refactoring e diagnostica.
- Garanzia della Qualità del Codice: Il file diagnostics.lua integra strumenti per il linting, la formattazione e l'analisi statica del codice, contribuendo a mantenere la qualità e la consistenza del codice.
- Estensione delle Funzionalità con Utilità: Il file utils.lua aggiunge una vasta gamma di utilità che semplificano e potenziano il workflow di sviluppo, dalla ricerca avanzata alla gestione delle sessioni, dei terminali, dei file e degli appunti.
- Ottimizzazione dell'Editing Markdown: Il file markdown.lua trasforma Neovim in un ambiente di editing Markdown efficiente e piacevole, con funzionalità avanzate di formattazione, anteprima e gestione delle tabelle.
