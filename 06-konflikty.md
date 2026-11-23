# 6. Konflikty

## Czym jest konflikt

Git potrafi **sam scalić** zmiany z dwóch gałęzi, o ile dotyczą **różnych miejsc** w plikach. Gdy **obie strony zmieniły ten sam
fragment tego samego pliku**, Git nie wie, którą wersję wybrać. Zatrzymuje scalanie i prosi **Ciebie** o decyzję.
To jest **konflikt scalania** (*merge conflict*).

> **Konflikt to nie awaria.** To normalna sytuacja w pracy zespołowej. Git nic nie zepsuł i nic nie stracił:
> po prostu pyta, co zostawić.

## Kiedy konflikt się zdarza

- przy `git merge` dwóch gałęzi,
- przy `git pull`, gdy ktoś zmienił ten sam fragment, który Ty zmieniłeś i zapisałeś u siebie (`pull` to „pobierz i scal"),
- przy `git stash pop` lub `git revert` (rzadziej).

## Przykład krok po kroku

Mamy plik `wspolny.txt` z linią „Wspolna". Na gałęzi `b1` zmieniamy ją na „Wersja z b1", a na `main` na „Wersja z main":

```
$ git switch -c b1
$ echo "Wersja z b1" > wspolny.txt
$ git commit -am "Zmiana na b1"

$ git switch main
$ echo "Wersja z main" > wspolny.txt
$ git commit -am "Zmiana na main"
```

Teraz scalamy `b1` do `main`:

```
$ git merge b1
Auto-merging wspolny.txt
CONFLICT (content): Merge conflict in wspolny.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Git **zatrzymał się w połowie**. Sprawdźmy stan:

```
$ git status
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   wspolny.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`both modified: wspolny.txt` oznacza, że **obie strony zmieniły ten plik**.

## Co Git zapisał w pliku

Otwórz `wspolny.txt`. Git wstawił **znaczniki konfliktu**:

```
<<<<<<< HEAD
Wersja z main
=======
Wersja z b1
>>>>>>> b1
```

| Fragment | Znaczenie |
|---|---|
| `<<<<<<< HEAD` | **początek** konfliktu. Poniżej jest wersja z **Twojej** (bieżącej) gałęzi, tu `main` |
| `=======` | **granica** między wersjami |
| `>>>>>>> b1` | **koniec** konfliktu. Powyżej jest wersja z **wciąganej** gałęzi `b1` |

## Jak rozwiązać konflikt

1. **Otwórz plik** w edytorze i **zdecyduj**, jaki ma być końcowy tekst. Możesz zostawić wersję z `main`, wersję z `b1`,
   obie albo napisać coś zupełnie nowego.
2. **Usuń wszystkie znaczniki** (`<<<<<<<`, `=======`, `>>>>>>>`). W pliku ma zostać tylko docelowy tekst.
3. Zapisz plik. Powiedzmy, że zostawiamy:

   ```
   Wersja po rozwiazaniu
   ```

4. Dodaj plik do poczekalni (to znaczy „konflikt rozwiązany") i zakończ scalanie commitem:

   ```
   $ git add wspolny.txt
   $ git commit --no-edit
   [main 350f151] Merge branch 'b1'
   ```

5. Sprawdź historię:

   ```
   $ git log --oneline --graph
   *   350f151 Merge branch 'b1'
   |\
   | * e79c2ba Zmiana na b1
   * | 105aacc Zmiana na main
   |/
   * c91ece6 Wspolny plik
   ```

> **Najczęstszy błąd:** zostawienie znaczników `<<<<<<<` w pliku i zrobienie commitu. Program przestanie się kompilować.
> Przed `git add` **zawsze przeszukaj plik** pod kątem `<<<<<<<`.

## Wyjście awaryjne: przerwij scalanie

Jeśli się pogubisz, możesz **wrócić do stanu sprzed `merge`**:

```
git merge --abort
```

Nic się nie stanie z Twoją pracą. Konflikt wróci przy następnej próbie scalenia.

## Konflikt przy `git pull`

To samo przydarza się przy pobieraniu zmian. Ala i Bartek pracują na tej samej gałęzi. Bartek wysłał zmianę w `README.md`,
a Ala zmieniła **ten sam plik** i próbuje wysłać swoją:

```
$ git push
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to '...'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. ...

$ git pull --no-rebase --no-edit
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Rozwiązujesz tak samo: edytujesz `README.md`, usuwasz znaczniki, `git add README.md`, `git commit`, a potem **`git push`**.

## Narzędzia pomagające

- **Visual Studio Code:** nad każdym konfliktem pojawiają się przyciski **Accept Current Change**, **Accept Incoming Change**,
  **Accept Both Changes** i widok porównania. Po rozwiązaniu zapisz plik, `git add` i `git commit`.
- **Visual Studio** i **GitHub Desktop** mają własne okna do rozwiązywania konfliktów (w GitHub Desktop pojawia się
  okno **Resolve conflicts**).
- **GitHub w przeglądarce:** przy pull requeście z konfliktem jest przycisk **Resolve conflicts**.

## Jak unikać konfliktów

- **Często pobieraj zmiany** (`git pull`) przed rozpoczęciem pracy.
- **Rób małe, częste commity** i szybko je wysyłaj.
- Ustal w zespole, **kto pracuje nad którym plikiem**.
- Nie zmieniaj jednocześnie formatowania całego pliku (to tworzy konflikty w każdej linii).

## Ćwiczenie

Ćwiczenie 7 (celowy konflikt i jego rozwiązanie): [cwiczenia.md](cwiczenia.md).

---

⬅️ [5. Gałęzie i scalanie](05-galezie-i-scalanie.md) · ➡️ [7. Zdalne repozytorium](07-zdalne-repozytorium.md)
