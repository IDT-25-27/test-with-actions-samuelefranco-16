## Passo 1: Continuous Integration

GitHub Actions è un ottimo modo per automatizzare diverse attività ricorrenti, risparmiando tempo per lavorare su problemi più impegnativi e divertenti!

Uno dei compiti più comuni che uno sviluppatore deve affrontare è il test del proprio codice. Purtroppo, questo è spesso noioso e le cose vengono saltate o semplicemente trascurate. Ancor di più, spesso dobbiamo testare contro molti framework, sistemi operativi e altre situazioni, esagerando il problema.

Impariamo come automatizzare questa crescente necessità di testare il nostro codice utilizzando i workflow in GitHub Actions.

> [!NOTE]
> Se vuoi saperne di più, controlla queste risorse:
> - [Understanding GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)
> - [Events that trigger workflows](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows)
> - [Actions runner pricing](https://docs.github.com/en/billing/reference/actions-runner-pricing)

### Cos'è la Continuous Integration?

La [Continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) può aiutarti a rispettare gli standard di qualità del tuo team eseguendo test e segnalando i risultati su GitHub. Gli strumenti di CI eseguono build e test, attivati dai commit. I risultati di qualità vengono riportati su GitHub nella pull request. L'obiettivo è avere meno problemi in `main` e un feedback più rapido mentre lavori.

### ⌨️ Attività: Avvia la nostra applicazione Python di esempio

1. Usa il pulsante qui sotto per aprire la pagina **Create Codespace** in una nuova scheda. Usa la configurazione predefinita.

   [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/{{full_repo_name}}?quickstart=1)

1. Conferma che il campo **Repository** sia la tua copia dell'esercizio, non l'originale, quindi fai clic sul pulsante verde **Create Codespace**.

   - ✅ Tua copia: `/{{full_repo_name}}`
   - ❌ Originale: `/skills/test-with-actions`

1. Attendi un momento che Visual Studio Code si carichi nel tuo browser.

1. Nella navigazione a sinistra, seleziona la scheda **Explorer** per mostrare i file del progetto.

1. Apri i file `src/calculations.py` e `tests/calculation_tests.py`.

1. Prenditi un momento per leggere questi file per familiarizzare.

1. Espandi il pannello terminale integrato di VS Code.

   > 💡 **Suggerimento**: La scorciatoia da tastiera è `CTRL` + `J`.

1. Esegui il comando qui sotto per creare un ambiente virtuale, quindi installa le librerie Python richieste e gli strumenti per mostrare la copertura del codice.

   ```bash
   python -m venv .venv/calculations
   source .venv/calculations/bin/activate
   pip install -r requirements.txt
   pip install pytest coverage pytest-cov
   ```

1. Esegui il comando qui sotto per eseguire tutti gli unit test e visualizzare le informazioni sulla copertura.

   ```bash
   pytest --cov=src --verbose
   ```

1. Aggiungi un commento in questa issue per far sapere a Mona i risultati del tuo report di copertura. Dopo la revisione, lei fornirà i passaggi successivi.

   ```md
   @professortocat, I've run my coverage report.
   Seems there is some opportunity to increase the test coverage. 🧐
   What should we do next?
   ```
