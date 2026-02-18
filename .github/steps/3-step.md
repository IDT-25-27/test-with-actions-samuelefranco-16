## Passo 3: Attiva i workflow

Come specificato nei nostri workflow, verranno eseguiti solo quando una pull request punta al branch `main`.

Le pull request hanno un bel vantaggio quando un workflow è associato a esse. Lo stato dell'esecuzione e i risultati possono essere visualizzati direttamente nel feed della conversazione.

### ⌨️ Attività: Avvia una PR e proponi una modifica al codice

1. Torna al Codespace di VS Code.

1. Crea un nuovo branch basato su `main` con il seguente nome e **pubblicalo** su GitHub.

   ```txt
   reenable-unit-test
   ```

1. Controlla di essere sul branch `reenable-unit-test`, quindi apri il file `tests/calculations_test.py`.

1. Dopo aver esaminato il codice, vediamo un test commentato alla riga 56. Rimuovi il commento per riabilitarlo.

   > Speriamo non sia stato disabilitato per evitare i test! 😱

   ```py
   def test_get_nth_fibonacci_ten():
    """Test with n=10."""
    # Arrange
    n = 10

    # Act
    result = get_nth_fibonacci(n)

    # Assert
    assert result == 89
   ```

1. Committa le modifiche e pushale su GitHub.

1. Torna al browser e crea una pull request. Usa i seguenti dettagli.

   - **base:** `main`
   - **source:** `reenable-unit-test`
   - **title**: `Reenable unit test that was disabled`

   <br/>
   <details>
   <summary><b>Hai problemi:</b> Non riesci a creare una pull request? 🤷‍♂️</summary>

   Hai committato accidentalmente nel branch `main`? Ecco un comando per annullare l'ultimo commit e aggiornare forzatamente il repository su GitHub. Fai attenzione a non tornare indietro di troppi passaggi però! Non vuoi rimuovere i tuoi nuovi workflow!

   ```bash
   git reset --hard HEAD~1
   git push -f
   ```

1. Dopo aver creato la pull request, guarda vicino al pulsante Merge per vedere molti workflow in esecuzione.

   - Il nostro workflow di copertura fallirà, facendoci sapere che abbiamo un test da correggere.

1. Con la pull request avviata, Mona dovrebbe essere impegnata a controllare il tuo lavoro e preparare i passaggi successivi.

<details>
<summary>Hai problemi?  🤷‍♂️</summary>

- Se i controlli non appaiono aggiornati, prova ad aggiornare la pagina. È possibile che il workflow sia stato eseguito e la pagina non sia stata ancora aggiornata con la modifica.

</details>
