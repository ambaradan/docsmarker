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
## Impostazione delle opzioni

### Introduzione

Una installazione standard di Neovim si presenta già pronta all'uso ma con un aspetto un po' semplice e certamente non ottimizzata per le vostre preferenze specifiche. Il primo passo per la personalizzazione dell'installazione passa dalla configurazione delle opzioni.  
Le opzioni di Neovim sono uno strumento potente e flessibile che consento di modificare numerosi aspetti dell'editor, dai più basilari come la visualizzazione dei numeri di riga nel buffer ad altre più avanzate come la sincronizzazione della clipboard del sistema.  
La loro flessibilità consente di applicarle a vari livelli, a livello globale per le impostazioni generali dell'editor, a livello di finestra per eventuali impostazioni di una area di lavoro e a livello di buffer.  

```text
vim.o = :set
vim.g = :setglobal
vim.bo = :setlocal (buffer options only)
vim.wo = :setlocal if global-local option, :set otherwise (window options only)
vim.wo[winid][bufid] = :setlocal (window options only)
vim.opt = :set
vim.opt_global= :setglobal
vim.opt_local = :setlocal
```
