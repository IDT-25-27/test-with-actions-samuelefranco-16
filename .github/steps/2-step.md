## Passo 2: File Workflow

Il modo migliore per aggiungere automazione al repository del tuo progetto è con un GitHub Actions Workflow. Diamo un'occhiata all'anatomia di un workflow, poi ne creiamo 2.

### Quali sono le parti di un workflow?

![Un'illustrazione con una metà sinistra e una destra. A sinistra: illustrazione di come i termini di GitHub Actions sono incapsulati. Al livello più alto: workflow e trigger di eventi. All'interno dei workflow: job e definizione dell'ambiente di build. All'interno dei job: step. All'interno degli step: una chiamata a un'action. A destra: la sequenza valutata: workflow, job, step, action.](https://github.com/IDT-25-27/idt-25-27-classroom-173794-test-with-actions-test-with-actions-1/blob/main/.github/images/github-actions-workflow-diagram.png?raw=true)

- **Workflow**: Un'unità di automazione dall'inizio alla fine. Inizia quando il trigger (`on`) corrisponde a un'attività nel repository. Consiste in 1 o più job.

- **Jobs**: I job del workflow vengono eseguiti ciascuno nei propri ambienti isolati e possono essere configurati in modo diverso. Vengono eseguiti in parallelo a meno che non sia configurato diversamente o siano impostate dipendenze.

- **Steps**: L'area degli step è una serie di action correlate che raggiungono l'obiettivo di un job. Uno step può essere un'_Action_ pre-fatta dall'Actions Marketplace, un'Action privata, uno script locale personalizzato o anche codice diretto.

- **Action**: Ogni step è un'_Action_, un pezzo di automazione scritto in modo compatibile con i workflow. Le Action possono essere scritte da GitHub, dalla comunità open source o specifiche per il progetto.

Nel file `Example Workflow` sopra, inizierà quando qualsiasi commit viene pushato nel repository su qualsiasi branch. Eseguirà 1 job con il nome `build`. Il primo step di quel job usa un'_Action_ pre-fatta dall'organizzazione `actions` chiamata `checkout` che clona il codice dal repository nell'ambiente del job.

Puoi esplorare tutte le opzioni di configurazione nella [GitHub Actions Docs](https://docs.github.com/actions/using-workflows/workflow-syntax-for-github-actions).

### ⌨️ Attività: Aggiungi un workflow per eseguire i test

1. Apri una scheda del browser web e naviga verso questo repository di esercizi. Il Codespace non è necessario in questo momento.

1. Nella navigazione in alto, seleziona la scheda **Actions**.

1. Nella navigazione a sinistra, sopra l'elenco dei workflow, fai clic sul pulsante **New workflow**.

   <img width="250" alt="immagine" src="https://github.com/IDT-25-27/idt-25-27-classroom-173794-test-with-actions-test-with-actions-1/blob/main/.github/images/new-workflow-button.png?raw=true" />

1. Inserisci `python package` nella casella di ricerca e fai clic sul pulsante **Enter**.

   <img width="300" alt="casella di ricerca con valore 'python package'" src="https://github.com/IDT-25-27/idt-25-27-classroom-173794-test-with-actions-test-with-actions-1/blob/main/.github/images/python-search-box.png?raw=true" />

1. Trova il workflow **Python package** e fai clic sul pulsante **Configure** per aprire un editor di file con un workflow pre-fatto. (Non selezionare quello che usa Anaconda)

   <img width="250" alt="immagine" src="https://github.com/IDT-25-27/idt-25-27-classroom-173794-test-with-actions-test-with-actions-1/blob/main/.github/images/python-package-configure.png?raw=true" />

1. Intorno alla riga 6, semplifica il trigger `on`. Rimuovi il trigger `push` e mantieni il trigger `pull_request`.

   ```yml
   on:
     pull_request:
       branches: ["main"]
   ```

1. Intorno alla riga 38, cambia il comando per mostrare più dettagli nei log.

   ```yml
   - name: Test with pytest
     run: |
       pytest --verbose
   ```

1. Sopra l'editor, a destra, fai clic sul pulsante **Commit changes...**. Committa direttamente nel branch `main`.

<details>
<summary>Hai problemi? 🤷‍♂️</summary>

L'indentazione dei file `.yml` è importante. Se ottieni errori di sintassi, quella potrebbe essere la ragione.

File workflow finito: `.github/workflows/python-package.yml.example`

</details>

### ⌨️ Attività: Aggiungi un workflow per mostrare la copertura dei test

1. Passa al Codespace di VS Code.

1. Controlla la barra di stato per un aggiornamento in sospeso. Cliccaci per pullare il tuo workflow committato di recente.

   <img width="130" alt="immagine" src="https://github.com/IDT-25-27/idt-25-27-classroom-173794-test-with-actions-test-with-actions-1/blob/main/.github/images/sync-changes-button.png?raw=true" />

1. Nella navigazione a sinistra, seleziona la scheda **Explorer** per mostrare i file del progetto.

1. Espandi la cartella `.github/workflows/`.

1. Aggiungi un nuovo file con il seguente nome e aprilo.

   ```txt
   python-coverage.yml
   ```

1. Inserisci il nome e impostalo per attivarsi sulle pull request che puntano al branch `main`.

   ```yml
   name: Python Coverage

   on:
     pull_request:
       branches:
         - main

   permissions:
     pull-requests: write
   ```

1. Aggiungi il job `python-coverage` e un primo step che ottiene il contenuto del repository.

   ```yml
   jobs:
     python-coverage:
       runs-on: ubuntu-latest

       steps:
         - name: Checkout code
           uses: actions/checkout@v4
   ```

1. Aggiungi step per installare Python e i pacchetti richiesti.

   ```yml
   - name: Set up Python
     uses: actions/setup-python@v4
     with:
       python-version: 3.13

   - name: Install dependencies
     run: |
       pip install -r requirements.txt
       pip install pytest==8.4.1
       pip install coverage==7.9
       pip install pytest-cov==6.2.1
   ```

1. Aggiungi step per eseguire il report di copertura sulla cartella `src`.

   ```yml
   - name: Run tests and generate coverage details
     run: pytest --cov=src
   ```

1. Aggiungi uno step finale che usa un'Action GitHub pre-fatta per condividere il report di copertura come commento sulla pull request.

   {% raw %}

   ```yml
   - name: Coverage comment
     uses: py-cov-action/python-coverage-comment-action@v3
     with:
       GITHUB_TOKEN: ${{ github.token }}
       MINIMUM_GREEN: 90
       MINIMUM_ORANGE: 70

   - name: Fail if below threshold
     run: coverage report --fail-under=90
   ```

   {% endraw %}

1. Committa e pusha le modifiche al file `python-coverage.yml` nel branch `main`.

1. Con entrambi i nuovi workflow pushati su GitHub, Mona revisionerà il tuo lavoro e posterà i passaggi successivi.

> [!TIP]
> Hai notato le molte dichiarazioni `uses:`? Sono step pre-fatti dall'[Actions Marketplace](https://github.com/marketplace?type=actions) gratuito. Considera di provarne uno prima di creare i tuoi script personalizzati (da mantenere)! Ci sono molte creazioni fantastiche dalla comunità!

<details>
<summary>Hai problemi? 🤷‍♂️</summary>

L'indentazione dei file `.yml` è importante. Se ottieni errori di sintassi, quella potrebbe essere la ragione.

File workflow finito: `.github/workflows/python-coverage.yml.example`

</details>
