---
title: Marksman
author: Franco Colussi
contributors: Steve Spencer
tags:
    - neovim
    - editor
    - markdown
---

<!--vale off-->
# Marksman

Marksman è un server linguistico che si integra con il vostro editor per aiutarvi a scrivere e mantenere i vostri documenti Markdown. Utilizzando il protocollo LSP, fornisce il completamento, la navigazione nella cartella di lavoro, la ricerca di riferimenti, il refactoring dei nomi, la diagnostica e altro ancora.

## Installazione

Marksman viene installato automaticamente durante la configurazione iniziale di Rocksmarker. Se non dovesse, per qualche motivo, essere disponibile può sempre essere installato manualmente con il comando:

```txt
:MasonInstall marksman
```

Per verificare la corretta installazione del server linguistico aprire un file markdown (*.md*) nell'editor e digitare il comando `:LspInfo`, si aprirà un buffer flottante contenente le informazioni descritte sotto, in caso sia mancante LspInfo non rileverà nessun client attaccato al buffer.

```txt
 Language client log: /home/ambaradan/.local/state/nvim/lsp.log
 Detected filetype:   markdown
 
 1 client(s) attached to this buffer: 
 
 Client: marksman (id: 1, bufnr: [1, 14])
  filetypes:       markdown, markdown.mdx
  autostart:       true
  root directory:  /home/ambaradan/.config/rocksmarker
  cmd:             /home/ambaradan/.local/share/nvim/mason/bin/marksman server
```

Il messaggio, come si può vedere, dice che nel buffer è stato rilevato un file di tipo markdown e che c'è un client attaccato (marksman).
Sono descritte le caratteristiche dei tipi di file supportati e che il server viene avviato automaticamente al rilevamento di quei tipi di file, segue poi l'indicazione della directory di lavoro e il comando utilizzato per il supporto linguistico.  
La direttiva `root directory` è molto importante in quanto indica la cartella che marksman usa per la diagnostica, la scrittura assistita di collegamenti e le altre funzionalità fornite dal server.

Questo implica che un file contenuto all'interno della cartella di lavoro, in questo caso la cartella rocksmarker, se aperto dalla cartella stessa viene controllato e supportato da marksman a livello di progetto:

```bash
cd .config/rocksmarker
nvim your_file.md
```

Mentre se aperto da una posizione fuori dalla root directory viene trattato da marksman al livello di file con la mancanza delle funzionalità proprie del progetto (come anteprima e gestione dei collegamenti, ricerca delle referenze e altre funzionalità):

```bash
nvim ~/.config/rocksmarker/your_file.md
```

La corretta implementazione del server linguistico è verificabile inoltre nella barra di stato dove viene visualizzato, se attaccato, il nome del server corrispondente.

![LSP info](./assets/img/lsp-info.png)

## Funzionalità di Marksman

### Navigazione assistita

#### Navigazione nel buffer

Marksman fornisce alcune utili scorciatoie per la navigazione dei buffer markdown, è possibile spostarsi fra le intestazioni del documento con la combinazione di due parentesi quadre. Con la combinazione ++"]]"++ si passa alla prossima intestazione mentre con la combinazione ++"[["++  si ritorna a quello precedente.

#### Navigazione nel workspace

Utilizzando invece la funzione **go to**, comune a tutti i server linguistici si può navigare i collegamenti interni al workspace, se il collegamento è interno al file, come nel caso di un TOC (Tabella dei contenuti), posizionandosi sul collegamento e richiamando la chiave da tastiera si viene automaticamente posizionati nella sezione corrispondente se invece il collegamento è esterno al file questo viene aperto in un nuovo buffer.  
La funzione è disponibile attraverso la chiave ++space+"g"+"d"++.

### Collegamenti

#### Auto completamento dei collegamenti

Il server linguistico supporta, nella scrittura assistita, l'auto completamento dei collegamenti. La funzionalità è molto utile in quanto permette di velocizzare la scrittura del documento e di evitare i problemi derivanti da percorsi scritti in modo errato.

!!! warning "Percorso assoluto o relativo"

    Marksman consente di utilizzare per i collegamenti sia i percorsi assoluti che quelli relativi ma va valutato attentamente l'uso di quelli assoluti. Il codice markdown viene elaborato da un *parser* ed è necessario controllare la correttezza del relativo codice finale HTML del collegamento. 

    #### Uso nella Documentazione Rocky Linux

    L'uso dei collegamenti assoluti per la scrittura di documenti per Rocky Linux è sconsigliata in quanto il progetto utilizza MkDocs per la trasformazione del codice markdown in codice statico HTML e nella documentazione relativa è presente il seguente avvertimento.

    > Using absolute paths with links is not officially supported. Relative paths are adjusted by MkDocs to ensure they are always relative to the page. Absolute paths are not modified at all. This means that your links using absolute paths might work fine in your local environment but they might break once you deploy them to your production server.

#### Collegamento con percorso assoluto

Durante la digitazione di un collegamento dopo aver inserito nella parentesi quadre il testo del collegamento alla digitazione delle due parentesi tonde si aprirà un pop-up contenente i nomi dei file presenti in quella area di lavoro che se selezionati forniranno un'ulteriore informazione sul titolo del file.  
Selezionando il file voluto con il tasto ENTER questo verrà automaticamente inserito nelle parentesi tonde.

![Absolute path](./assets/img/marksman-absolute-path.png)

#### Collegamento con percorso relativo

Il collegamento ad un percorso relativo nella gestione assistita è invece attivato dalla digitazione del punto di partenza `./` che come per il percorso assoluto presenta una lista delle cartelle e dei file presenti nell'area di lavoro. In questo caso però il nome del file da collegare è preceduto dalla sua posizione rispetto alla cartella dove si trova il file e la sua selezione offre un'anteprima del documento che si va a collegare agevolando in questo modo il lavoro.  
Questo tipo di collegamento ha inoltre il vantaggio di consentire nel collegamento la risalita dalla cartella dove si trova il file usando il percorso `../` permettendo così una gestione multi cartella del progetto.

![Relative path](./assets/img/marksman-relative-path.png)

#### Verifica collegamenti

Marksman integra nella gestione dei collegamenti anche un controllo della presenza del documento corrispondente. Questo è un ottimo aiuto nella gestione di progetti particolarmente ricchi di documenti come ad esempio la documentazione su Rocky Linux.
Permette di evitare errori di battitura o di distrazione solitamente difficili da individuare.

![Links check](./assets/img/marksman-check-link.png)

!!! tip ""

    La visualizzazione estesa degli errori della schermata precedente è fornita dal plugin *trouble.nvim* attivabile con la scorciatoia ++space+"t"+"b"++

#### Anteprima dei collegamenti

Per i collegamenti già presenti nel documento è possibile visualizzare un'anteprima del contenuto del file. Questa funzionalità risulta particolarmente utile nella revisione di documenti datati dove non sempre si ricordano i contenuti dei file collegati.  
Per attivare l'anteprima posizionare il cursore sul collegamento desiderato e digitare ++"K"++ (maiuscola), per chiuderla basta muovere il cursore.

![Link preview](./assets/img/marksman-link-preview.png)

!!! note ""

    L'anteprima dei collegamenti è disponibile solo per i collegamenti riferiti a file presenti nella cartella di lavoro, non vengono fornite anteprime per i collegamenti web e per file esterni al *workspace*.

#### Rinominare e ristrutturare i collegamenti

Il server linguistico consente anche di rinominare a livello di workspace le intestazioni presenti nei documenti, nel rinominare marksman gestisce anche gli eventuali collegamenti di riferimento presenti assicurando così la corretta navigazione.  
Per la sua attivazione è fornita la chiave ++space+"r"+"n"++, la sua digitazione apre nella barra di stato un messaggio ==:New Name== seguito dal testo della intestazione dove si trova il cursore, modificando il testo viene modificato l'intestazione e tutti i collegamenti di riferimento per quella intestazione.

!!! info "Documentazione Rocky"

    I collegamenti di riferimento nei documenti scritti per la documentazione su Rocky Linux vanno evitati, questo perché non compatibili con l'ambiente di sviluppo utilizzato. La natura multilingua del sito e la relativa elaborazione da parte del motore di traduzione Crowdin non permette la traduzione di questi tipi di collegamento.

### Code Action

Una CodeAction rappresenta una modifica o un comando che può essere eseguito sul codice, ad esempio per risolvere un problema o per ristrutturarlo. Marksman dispone di una code action per la gestione dei TOC (tabella dei contenuti), per la sua creazione basta posizionare il cursore nella posizione desiderata e digitare la chiave ++space+"c"+"a"++ e comparirà il pop-up seguente:

![Marksman TOC action](./assets/img/marksman-toc.png)

Selezionando la code action viene creata la tabella dei collegamenti di riferimento che successivamente può essere modificata e gestita con la funzione di ristrutturazione descritta sopra (++space+"r"+"n"++).  
Un esempio di quanto descritto è il TOC del README del repository del progetto Rocksmarker:

![Marksman TOC Code](./assets/img/marksman-toc-code.png)

!!! info "Documentazione Rocky"

    Anche questa funzionalità come quella descritta in precedenza non è adatta per i documenti scritti per la documentazione su Rocky Linux. La navigazione dei riferimenti dei documenti viene gestita automaticamente dal plugin *mkdocs-material* di MkDocs rendendolo di fatto inutile.

## Conclusione

Anche se non è strettamente necessario, questo server linguistico può diventare, col tempo, un ottimo compagno nella scrittura della documentazione per Rocky Linux.  
Il suo utilizzo consente di risparmiare tempo nella costruzione della struttura delle pagine e di evitare errori banali come una digitazione errata.
