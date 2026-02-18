## Passo 4: Imponi i workflow

Potresti aver notato che il pulsante merge era ancora attivo prima che i nostri test finissero.
Peggio ancora, alcuni test sono falliti e non c'era nulla che impedisse di unire comunque il codice rotto! 😱

Risolviamo questo problema per evitare che qualcuno (accidentalmente) aggiri la verifica.

### ⌨️ Attività: Aggiungi branch protection

1. Nella navigazione in alto, seleziona la scheda **Settings**.

1. Nella navigazione a sinistra, seleziona **Rules** e scegli **Rulesets**.

1. Fai clic su **New ruleset** e seleziona **New branch ruleset**. Usa le seguenti impostazioni:

   - **Ruleset Name:** `Protect main`
   - **Enforcement status:** `Active`
   - **Target branches:** (Aggiungi Target)
     - **Include default branch**
     - **Include by pattern:** `main`
   - **Require status checks to pass**: ☑️ (Aggiungi Check)
     - `python-coverage`

   <br/>

   > 🪧 **Nota:** Per mantenere la lezione semplice, stiamo controllando solo il workflow di copertura. Sentiti libero di sperimentare però!

   <img width="300" alt="target branch settings" src="https://github.com/IDT-25-27/idt-25-27-classroom-173794-test-with-actions-test-with-actions-1/blob/main/.github/images/branch-protection-target-settings.png?raw=true" />

   <img width="300" alt="required status checks" src="https://github.com/IDT-25-27/idt-25-27-classroom-173794-test-with-actions-test-with-actions-1/blob/main/.github/images/required-status-checks.png?raw=true" />

1. Fai clic su **Create**.

1. Torna alla pull request e aggiorna la pagina.

1. Scorri fino in fondo per trovare i workflow falliti e il pulsante **Merge** è ora disabilitato! Ottimo! 🥰

   <img width="500" alt="failed tests and disabled merge button" src="https://github.com/IDT-25-27/idt-25-27-classroom-173794-test-with-actions-test-with-actions-1/blob/main/.github/images/failed-tests-disabled-merge.png?raw=true" />

> [!TIP]
> Interessato a imparare altri modi per preparare il tuo progetto per la collaborazione? Dai un'occhiata all'esercizio [Introduction to Repository Management](https://github.com/skills/introduction-to-repository-management) dopo!

### Attività: Correggi il test rotto

Indaghiamo perché il nostro workflow di test è fallito. È mal configurato o qualche codice è errato? Forse c'era un motivo per cui quel test era disabilitato?!

1. Fai clic sul workflow `Python Coverage` per visualizzare i log. Navigherà automaticamente ai log falliti.

1. Dopo qualche ispezione, ci sono 2 problemi che impediscono il merge.

   - 1 test sta fallendo.
   - La copertura è inferiore al requisito del 90%.

1. Passa al Codespace di VS Code.

1. Apri il file `tests/calculations_test.py`.

1. Dopo qualche indagine, vediamo che il test rotto potrebbe essere stato commentato perché era progettato in modo errato.

   - Una rapida ricerca su google mostra che la decima voce nella sequenza di Fibonacci è `55`, non `89`.

1. Modifica il test per usare il valore di assert corretto.

   ```bash
   def test_get_nth_fibonacci_ten():
      """Test with n=10."""
      # Arrange
      n = 10

      # Act
      result = get_nth_fibonacci(n)

      # Assert
      assert result == 55
   ```

1. Committa e pusha il codice di test corretto, quindi attendi che i workflow vengano eseguiti di nuovo.

   - Questa volta i test passano e riceviamo un report di copertura dettagliato.

## Attività: Correggi la bassa copertura dei test

Con il nostro test corretto, ora stiamo ottenendo risultati di copertura.
Purtroppo è inferiore al requisito del 90%.
Aggiungiamo altri test per aumentare la copertura.

1. Aggiungiamo 2 test per aumentare la copertura.
In alternativa, puoi chiedere a GitHub Copilot di trovare casi di test mancanti espandendo il prompt qui sotto.


   1. Apri il file `tests/calculations_test.py`.

   1. Aggiungi le seguenti 2 voci.

      ```py
      def test_area_of_circle_negative_radius():
         """Test with a negative radius to raise ValueError."""
         # Arrange
         radius = -1

         # Act & Assert
         with pytest.raises(ValueError):
            area_of_circle(radius)
      ```

      ```py
      def test_get_nth_fibonacci_negative():
         """Test with a negative number to raise ValueError."""
         # Arrange
         n = -1

         # Act & Assert
         with pytest.raises(ValueError):
            get_nth_fibonacci(n)
      ```

   <details>
   <summary>🪧 <b>Mostra:</b> Prompt</summary>

   > ![Static Badge](https://img.shields.io/badge/-Prompt-text?style=social&logo=github%20copilot)
   >
   > ```prompt
   > Hey Copilot, the test coverage is too low. Please find the missing tests to get us to 100% coverage.
   > ```

   </details>

1. Committa e pusha i 2 nuovi test.

1. Attendi un momento che i workflow vengano eseguiti un'ultima volta.

   - Il commento sulla copertura si aggiornerà al 100%.
   - Il pulsante merge si attiverà!

1. Fai clic sul pulsante **Merge**.

   <img width="500" alt="immagine" src="https://github.com/IDT-25-27/idt-25-27-classroom-173794-test-with-actions-test-with-actions-1/blob/main/.github/images/merge-button-active.png?raw=true" />

1. Con la copertura completa, tutti i test passati e la pull request unita, Mona condividerà una revisione finale. Congratulazioni, hai finito!
