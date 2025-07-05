---
title: Markdownlint
author: Steven Spencer
contributors: Franco Colussi
tags:
    - neovim
    - editor
    - markdown
---
<!--vale off-->

## Introduzione

### Cos'è Markdownlint?

Markdownlint è uno strumento di linting progettato per analizzare e validare file scritti in Markdown, assicurandosi che rispettino un insieme di regole di formattazione coerenti. Questo strumento è particolarmente utile per:

- Migliorare la leggibilità dei documenti Markdown.
- Mantenere uno stile uniforme tra più file o collaboratori.
- Evitare errori comuni nella sintassi Markdown.

### Perché usare Markdownlint?

Scrivere documentazione o contenuti in Markdown è semplice, ma senza linee guida chiare, possono emergere inconsistenze come:

- Spaziature incoerenti
- Intestazioni mal formattate
- Liste non allineate
- Link o immagini con sintassi errata

Markdownlint aiuta a identificare e correggere questi problemi automaticamente, risparmiando tempo e migliorando la qualità del codice.

### Come funziona?

Markdownlint funziona applicando un insieme di regole configurabili per garantire che i documenti siano leggibili, ben strutturati e conformi agli standard. I passaggi gestiti dal linter sono i seguenti:

: **Analisi del codice markdown**

: Markdownlint esamina il testo alla ricerca di pattern che corrispondono a:

    - Sintassi non valida (es. intestazioni malformate, link rotti).
    - Stile incoerente (es. uso misto di - e * per le liste).
    - Errori comuni (es. spazi mancanti dopo i # nelle intestazioni).

: **Applicazione delle regole**

: Lo strumento utilizza un set di regole predefinite, ciascuna identificata da un codice (es. MD001, MD037).

    - Alcune regole sono automaticamente correggibili (es. aggiunta di spazi mancanti).
    - Altre richiedono intervento manuale (es. suddivisione di righe troppo lunghe).

: **Output dei risultati**

: Markdownlint genera report con:

    - Posizione degli errori (file, riga, colonna).
    - Descrizione del problema.
    - Codice della regola violata (per riferimento rapido).

: **Integrazione con workflow**

: Può essere eseguito in:

    - Editor (VS Code, Neovim) con highlight in tempo reale.
    - CLI (tramite markdownlint-cli) per verifiche batch.
    - Pipeline CI/CD (es. GitHub Actions) per bloccare merge con errori.

## Il file `.markdownlint.yaml`

### Cos'è il file .markdownlint.yaml?

Il file .markdownlint.yaml è un file di configurazione che permette di personalizzare il comportamento di Markdownlint, specificando quali regole applicare e con quali parametri. A differenza del formato JSON (.markdownlint.json), il formato YAML offre:

- Una sintassi più leggibile e concisa
- Supporto nativo ai commenti
- Maggiore flessibilità nell'organizzazione delle regole

Rocksmarker include il file delle regole per il linting, `.markdownlint.yaml`, nella radice del progetto. È possibile trovare questo file anche in altri progetti, anche se le regole in esso contenute potrebbero essere diverse. La documentazione di Rocky Linux include un file `.markdownlint.yaml` con contenuti molto simili a quelli di Rocksmarker. Le regole in uso al momento in cui scriviamo provengono dall'ultima versione (v0.37.4) di [David Anson's rule set](https://github.com/DavidAnson/markdownlint).

### Spiegazione delle regole

Rocksmarker mantiene i valori predefiniti per la maggior parte delle regole nel file `.markdownlint.yaml`. Ci sono alcune eccezioni.

| Regola | Spiegazione                                                                                                     | Valore                              |
|--------|-----------------------------------------------------------------------------------------------------------------|-------------------------------------|
| MD001  | Garantisce l'incremento delle intestazioni. Non è consentito saltare da h1 a h3.                                | default                             |
| MD003  | Garantisce lo stile delle intestazioni. Rocky Linux supporta comunque solo intestazioni in stile ATX.           | default                             |
| MD004  | Lo stile degli elenchi non ordinati deve essere coerente. (usare solo - o \*, ma non entrambi).                 | default                             |
| MD005  | Indentazione incoerente degli elementi dell'elenco. Impone una corretta indentazione di tutti gli elenchi.      | default                             |
| MD007  | Indentazione degli elenchi non ordinati. Spazi per l'indentazione.                                              | spazi per il rientro modificati a 4 |
| MD009  | Spazi intermedi. Imposta il valore consentito per gli spazi intermedi.                                          | default                             |
| MD010  | Garantisce l'assenza di tabulazioni o spazi bianchi. Assicura gli spazi solo per l'indentazione.                | default                             |
| MD011  | Garantisce la corretta formattazione dei link.                                                                  | default                             |
| MD012  | Consente solo una singola riga vuota tra gli elementi.                                                          | default                             |
| MD013  | Regole sulla lunghezza delle linee.                                                                             | disabled                            |
| MD014  | Nessun \$ che precede il codice che non viene visualizzato.                                                     | default                             |
| MD018  | Impone lo spazio dopo il cancelletto in un'intestazione ATX.                                                    | default                             |
| MD019  | Garantisce un solo spazio dopo l'hash in un'intestazione ATX.                                                   | default                             |
| MD020  | Garantisce gli spazi in corrispondenza dell'intestazione ATX chiusa prima del cancelletto. Non utilizzato.      | default                             |
| MD021  | Per le intestazioni ATX chiuse. Non utilizzato.                                                                 | default                             |
| MD022  | Garantisce le intestazioni adiacenti con una riga vuota.                                                        | default                             |
| MD023  | Garantisce che le intestazioni inizino all'inizio della riga. (nessuna indentazione)                            | default                             |
| MD024  | Assenza di titoli duplicati.                                                                                    | default                             |
| MD025  | Impone un solo titolo di livello superiore (h1) in un documento.                                                | default                             |
| MD026  | Nessuna punteggiatura finale nell'intestazione.                                                                 | default                             |
| MD027  | Impone un solo spazio dopo il simbolo delle virgolette (`>`).                                                   | default                             |
| MD028  | Nessuna riga vuota all'interno di una citazione a blocchi.                                                      | default                             |
| MD029  | Prefisso dell'elenco ordinato.                                                                                  | default                             |
| MD030  | Impone un unico spazio dopo i marcatori di elenco.                                                              | default                             |
| MD031  | Garantisce linee vuote intorno ai blocchi di codice.                                                            | default                             |
| MD032  | Garantisce una riga vuota intorno agli elenchi di qualsiasi tipo.                                               | default                             |
| MD033  | Nessun HTML in linea. Rocky Linux visualizza gli errori quando si utilizzano elementi HTML.                     | default                             |
| MD034  | Nessun URL puro. Per utilizzare un URL al di fuori di un link, è necessario circondarlo con (`<>`).             | default                             |
| MD035  | Lo stile delle righe orizzontali deve essere coerente.                                                          | default                             |
| MD036  | Non utilizzare l'enfasi per sostituire un'intestazione con (`**il mio testo**`).                                | default                             |
| MD037  | Niente spazi nell'enfasi. Quando si utilizza il formato grassetto o corsivo senza spazi (`*questo*`)            | default                             |
| MD038  | Nessuno spazio nel codice (backtick singolo senza spazi)                                                        | default                             |
| MD039  | Nessun spazio all'interno del testo del link.                                                                   | default                             |
| MD040  | I blocchi di codice delimitati devono avere un linguaggio specifico (bash, text, markdown, ecc.).               | default                             |
| MD041  | La prima riga del file deve essere un titolo di livello superiore.                                              | default                             |
| MD042  | NNessun collegamento vuoto.                                                                                     | default                             |
| MD043  | Struttura dell'intestazione richiesta. Non applicabile.                                                         | default                             |
| MD044  | Maiuscola del nome proprio. (gestita da `vale` in questo caso, quindi ignorata).                                | default                             |
| MD045  | Tutte le immagini devono avere un testo alternativo.                                                            | default                             |
| MD046  | Blocchi di codice coerenti. Rocky Linux utilizza blocchi di codice delimitati.                                  | default                             |
| MD047  | I file devono terminare con un singolo carattere di newline.                                                    | default                             |
| MD048  | Utilizzare uno stile coerente per i blocchi di codice delimitati.                                               | default                             |
| MD049  | Uso di uno stile di enfasi coerente. (corsivo)                                                                  | default                             |
| MD050  | Uso di uno stile di enfasi coerente. (grassetto)                                                                | default                             |
| MD051  | Collegamenti all'interno del documento. Non applicabile. Non utilizzare collegamenti all'interno dei documenti. | default                             |
| MD052  | Riguarda i collegamenti e le etichette di riferimento. Non si applica a Rocky Linux.                            | default                             |
| MD053  | Necessità di collegamenti e definizioni di riferimento.                                                         | default                             |
| MD054  | Stile dei link e delle immagini. Definito da material-mkdocs in Rocky Linux. Ignorato.                          | default                             |
| MD055  | Utilizzare uno stile di tabella coerente. Forzato all'interno di material-mkdocs. Ignorato.                     | default                             |
| MD056  | Il numero di colonne della tabella deve corrispondere.                                                          | default                             |
| MD058  | Garantisce le righe vuote intorno alle tabelle.                                                                 | default                             |

La modifica degli spazi di rientro con la regola **MD007** serve per un corretto rientro all'interno delle ammonizioni. La documentazione di Rocky Linux non richiede regole di lunghezza delle righe (**MD013**).

Quando si infrange una regola di markdownlint, si riceve un errore con il numero della regola:

![markdownlint_rule_error](../assets/img/interface_with_rule_tripped.png)

## Integrazione nell'editor

Le regole di *Markdownlint* vengono utilizzate dall'editor sia per la funzione di segnalazione degli errori nella configurazione del plugin *nvim-lint* che per la formattazione del codice in *conform.nvim*.  
La combinazione dei due controlli permette di ridurre al minimo gli errori da correggere, il formattatore corregge ad ogni salvataggio gli errori rimediabili automaticamente mentre il linter segnala quelli che per la loro correzione hanno bisogno dell'intervento manuale.

### Formattatore

Come accennato in precedenza il plugin *conform.nvim* fornisce la formattazione del codice, la sua impostazione è per tipo di file (==formatters_by_ft==) e come si può vedere dal codice sottostante fornisce il supporto per molti linguaggi di programmazione.  
La funzione è impostata per essere richiamata ad ogni salvataggio del buffer (==format_on_save==) e quando richiamata usa le regole fornite da *markdownlint*.  
In questo modo tutti gli errori rilevati ma recuperabili vengono automaticamente corretti senza la necessità di interventi manuali.

```lua title="/lua/plugins/diagnostic.lua" linenums="2" hl_lines="2 8 12"
require("conform").setup({
 formatters_by_ft = {
  lua = { "stylua" },
  css = { "prettier" },
  html = { "prettier" },
  sh = { "shfmt" },
  bash = { "shfmt" },
  markdown = { "markdownlint" },
  yaml = { "yamlfmt" },
 },

 format_on_save = {
  -- These options will be passed to conform.format()
  timeout_ms = 1000,
  lsp_format = "fallback",
 },
})
```

#### Formattazione manuale

Per avere il pieno controllo degli errori markdown del buffer è possibile impostare la formattazione manuale. Per farlo è sufficiente commentare la parte relativa a **format_on_save** e riavviare l'editor.

```lua title="codice da formattare" linenums="13"
 --format_on_save = {
  -- These options will be passed to conform.format()
  --timeout_ms = 1000,
  --lsp_format = "fallback",
 --},
```

L'editor in questa configurazione segnalerà tutti gli errori markdown nel buffer senza applicare alcuna correzione, questo può essere didatticamente interessante per apprendere le regole e correggere il proprio modo di scrivere.  
Gli errori saranno presenti anche dopo il salvataggio del buffer consentendo così di studiarli e verificare manualmente le modifiche.

!!! tip ""

    La formattazione automatica è comunque disponibile e può essere applicata con la scorciatoia ++space+"F"++. La chiave richiama la stessa funzione utilizzata per la formattazione da ==format_on_save==.

    ```lua title="/lua/mappings.lua" linenums="70"
    {
    "<leader>F",
     function()
      require("conform").format({ lsp_fallback = true })
     end,
    desc = "format buffer",
    mode = "n",
    },
    ```

### Linter

La funzione di linting è affidata al plugin *nvim-lint* un linter asincrono estremamente efficiente e complementare al supporto fornito dal protocollo linguistico di Neovim. Può essere configurato per il controllo di un gran numero di linguaggi di programmazione, i linguaggi disponibili sono consultabili nella [sezione relativa](https://github.com/mfussenegger/nvim-lint?tab=readme-ov-file#available-linters) del progetto.  
La funzionalità viene attivata da un auto comando attivato dall'evento **BufWritePost**, evento che si verifica ad ogni scrittura del buffer sul disco rigido. Anche in questo caso la verifica del codice è definita per tipo di file (==linters_by_ft==) e nel caso dei documenti markdown viene verificata sia la correttezza del codice con *markdownlint* che del contenuto con *vale*.

```lua title="/lua/plugins/diagnostic.lua" linenums="23" hl_lines="2 7"
require("lint").linters_by_ft = {
 markdown = { "markdownlint", "vale" },
 yaml = { "yamllint" },
 bash = { "shellcheck" },
}

vim.api.nvim_create_autocmd({ "BufWritePost" }, {
 callback = function()
  require("lint").try_lint()
 end,
})
```

## Conclusioni
