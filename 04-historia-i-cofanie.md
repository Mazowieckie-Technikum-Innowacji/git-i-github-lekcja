# 4. Historia i cofanie zmian

Błędy są normalne: źle zmieniony plik, commit z literówką, zapisany plik, którego nie chciałeś. Git daje na to **kilka
narzędzi**, a wybór zależy od tego, **w którym z trzech obszarów** jest zmiana.

## Którego polecenia użyć? (ściągawka)

| Sytuacja | Polecenie |
|---|---|
| Zmieniłem plik i chcę **cofnąć zmiany** (jeszcze nie ma commitu) | `git restore plik` |
| Dodałem plik do poczekalni przez pomyłkę | `git restore --staged plik` |
| Ostatni commit miał literówkę w opisie / brakowało pliku (**jeszcze nie wysłany**) | `git commit --amend` |
| Chcę **wycofać** stary commit, ale zostawić historię (**bezpieczne, także po `push`**) | `git revert` |
| Chcę **cofnąć** commity lokalnie (**jeszcze nie wysłane**) | `git reset` |
| Chcę na chwilę odłożyć niedokończoną pracę | `git stash` |
| Skasowałem coś przez `reset --hard` | `git reflog` |

> **Złota zasada:** commitów, które **już wysłałeś** na GitHub (`push`), **nie przepisuje się** poleceniami `reset` i `amend`.
> Inni mogą je mieć u siebie. Do cofania wysłanych commitów używamy **`git revert`**.

---

## `git restore`: cofnięcie zmian w pliku

**Cofnięcie zmian w katalogu roboczym** (przywraca plik do stanu z ostatniego commitu):

```
$ git status --short
 M hello.txt

$ git restore hello.txt

$ git status
On branch main
nothing to commit, working tree clean
```

> ⚠️ **To polecenie jest nieodwracalne.** Zmiany, których nie zapisałeś w commicie ani w poczekalni, **znikają na zawsze**
> (nie ma ich w koszu). Zanim je wywołasz, sprawdź `git diff`, czy na pewno chcesz je porzucić.

**Wycofanie z poczekalni** (plik zostaje zmieniony, ale nie wejdzie do commitu):

```
$ git add notatki.txt
$ git restore --staged notatki.txt
$ git status --short
?? notatki.txt
```

**Przywrócenie pliku do starszej wersji** z wybranego commitu:

```
$ git restore --source=HEAD~2 plik.txt
$ cat plik.txt
wersja 1
```

(`--source` wskazuje commit, z którego bierzemy zawartość pliku. Zmiana jest w katalogu roboczym, więc nadal trzeba
zrobić `add` i `commit`.)

| Polecenie | Z czego przywraca | Dokąd |
|---|---|---|
| `git restore plik` | z poczekalni (lub `HEAD`) | do katalogu roboczego |
| `git restore --staged plik` | z `HEAD` | do poczekalni |
| `git restore --source=<commit> plik` | z wybranego commitu | do katalogu roboczego |

> **Starszy zapis:** w starszych poradnikach zobaczysz `git checkout -- plik` albo `git reset HEAD plik`. Robią to samo,
> ale `git restore` jest czytelniejszy (dostępny od Gita 2.23).

---

## `git commit --amend`: poprawienie ostatniego commitu

Zrobiłeś commit i zauważyłeś literówkę w opisie albo zapomniałeś dodać plik. **Zamiast** robić nowy commit, możesz poprawić
ostatni:

**Zmiana opisu:**

```
$ git commit --amend -m "Dopisano druga linie"
[main 08fe28b] Dopisano druga linie
 Date: Tue Nov 24 10:00:00 2026 +0100
 1 file changed, 1 insertion(+)
```

**Dodanie zapomnianego pliku** (bez zmiany opisu):

```
$ git add b.txt
$ git commit --amend --no-edit
[main 34a4545] Dodano a
 Date: Tue Nov 24 10:00:00 2026 +0100
 2 files changed, 2 insertions(+)
 create mode 100644 a.txt
 create mode 100644 b.txt
```

Zwróć uwagę, że **identyfikator commitu się zmienił** (`b3a17b0` → `08fe28b`). `--amend` nie edytuje starego commitu, tylko
**tworzy nowy na jego miejsce**. Dlatego nie używaj go po `push`.

---

## `git reset`: cofnięcie commitów (lokalnie)

`git reset` przesuwa wskaźnik gałęzi (i `HEAD`) na **wcześniejszy commit**. Są **trzy tryby**, różniące się tym, co
robią ze zmianami z „cofniętych" commitów:

| Tryb | Co robi z commitem | Co ze zmianami z niego |
|---|---|---|
| `--soft` | cofa commit | zmiany **zostają w poczekalni** (gotowe do nowego commitu) |
| `--mixed` (domyślny) | cofa commit | zmiany **zostają w plikach**, ale **poza poczekalnią** |
| `--hard` | cofa commit | zmiany **znikają** (katalog roboczy wraca do tego commitu) |

Przykład, krok po kroku. **Dla każdego trybu zaczynamy od tych samych trzech commitów** (po każdym
przykładzie wróć do tego stanu, albo użyj osobnego, świeżego repozytorium):

```
$ git log --oneline
4dbf4df Commit 3
7fab43a Commit 2
59b0c20 Commit 1
```

**`--soft`**: cofamy ostatni commit, zmiana zostaje w poczekalni:

```
$ git reset --soft HEAD~1
$ git status --short
M  plik.txt          ← zmiana nadal czeka w poczekalni (litera M w pierwszej kolumnie)
```

Przydatne, gdy chcesz **rozbić lub połączyć** commity albo poprawić opis (zrobisz `commit` jeszcze raz).

**`--mixed`** (domyślny):

```
$ git reset HEAD~1
Unstaged changes after reset:
M	plik.txt
$ git status --short
 M plik.txt          ← zmiana jest w pliku, ale nie w poczekalni
```

**`--hard`**: wszystko wraca do starego stanu, zmiany **przepadają**:

```
$ git reset --hard HEAD~1
HEAD is now at 7fab43a Commit 2
$ git log --oneline
7fab43a Commit 2
59b0c20 Commit 1
```

> ⚠️ **`git reset --hard` kasuje niezapisane zmiany w plikach bez pytania.** Używaj go świadomie. Jeśli pomyliłeś się
> po cofnięciu **samych commitów**, da się je odzyskać przez `git reflog` (niżej).

`HEAD~1` oznacza „o jeden commit wcześniej niż `HEAD`". Można też podać skrót commitu: `git reset --hard 7fab43a`.

---

## `git revert`: wycofanie commitu przez nowy commit

`revert` **nie kasuje** historii. Tworzy **nowy commit, który odwraca zmiany** wskazanego commitu. Dlatego jest **bezpieczny
także dla commitów wysłanych na GitHub**.

```
$ git log --oneline
542acd0 Zly commit
bbea9c0 Dodano notatki
...

$ git revert --no-edit HEAD
[main 76417d5] Revert "Zly commit"
 Date: Tue Nov 24 10:00:00 2026 +0100
 1 file changed, 1 deletion(-)

$ git log --oneline
76417d5 Revert "Zly commit"
542acd0 Zly commit
bbea9c0 Dodano notatki
...
```

| Opcja | Znaczenie |
|---|---|
| `git revert HEAD` | wycofuje ostatni commit (Git otworzy edytor, żebyś potwierdził opis) |
| `git revert 542acd0` | wycofuje wskazany commit |
| `--no-edit` | użyj domyślnego opisu (`Revert "…"`) bez otwierania edytora |

**Różnica `reset` kontra `revert`:**

| | `reset` | `revert` |
|---|---|---|
| Historia | **przepisuje** (commit znika) | **dopisuje** (nowy commit odwracający) |
| Po `push` | **niebezpieczne** | **bezpieczne** |
| Kiedy | lokalne porządki | wycofanie błędu, który już jest na GitHubie |

---

## `git stash`: odłóż pracę „do szuflady"

Pracujesz nad czymś, nagle trzeba przełączyć się na inną gałąź albo zrobić `pull`, a Ty masz niedokończone zmiany, których
nie chcesz jeszcze zapisywać. Użyj `stash`.

```
$ git status --short
 M plik.txt

$ git stash
Saved working directory and index state WIP on main: 7fab43a Commit 2

$ git status --short
                    ← pusto: katalog roboczy jest czysty

$ git stash list
stash@{0}: WIP on main: 7fab43a Commit 2

$ git stash pop
On branch main
Changes not staged for commit:
	modified:   plik.txt
Dropped refs/stash@{0} (22d09ed...)
```

| Polecenie | Co robi |
|---|---|
| `git stash` | chowa zmiany śledzonych plików i czyści katalog roboczy |
| `git stash list` | pokazuje listę schowanych zmian |
| `git stash pop` | przywraca ostatnio schowane zmiany **i usuwa je ze schowka** |
| `git stash apply` | przywraca zmiany, ale **zostawia kopię** w schowku |
| `git stash drop` | usuwa ostatni wpis ze schowka |
| `git stash -u` | chowa także **nowe (nieśledzone)** pliki |

---

## `git tag`: etykieta na ważnym commicie

Tag to **stała nazwa** przypisana do commitu, np. oznaczenie wersji projektu.

```
$ git tag v1.0
$ git tag
v1.0
```

| Polecenie | Co robi |
|---|---|
| `git tag v1.0` | tworzy lekki tag na bieżącym commicie |
| `git tag -a v1.0 -m "Pierwsza wersja"` | tworzy tag z opisem (**adnotowany**) |
| `git tag` | wypisuje tagi |
| `git push origin v1.0` | wysyła tag na GitHub (zwykłe `push` **nie** wysyła tagów) |
| `git push origin --tags` | wysyła wszystkie tagi |

W Classroom 50 niektóre zadania mogą liczyć jako „oddanie" **tag** zamiast zwykłego `push` (o tym mówi opis zadania).

---

## `git reflog`: siatka bezpieczeństwa

Git zapisuje, **gdzie wskazywał `HEAD`** w ostatnim czasie, nawet jeśli commity zniknęły z historii po `reset --hard`.
Dzięki temu można je odzyskać.

```
$ git log --oneline
89a7598 Commit 3
c69b4da Commit 2
ccd01ae Commit 1

$ git reset --hard HEAD~2          ← „przez pomyłkę" cofnęliśmy dwa commity
HEAD is now at ccd01ae Commit 1

$ git reflog
ccd01ae HEAD@{0}: reset: moving to HEAD~2
89a7598 HEAD@{1}: commit: Commit 3
c69b4da HEAD@{2}: commit: Commit 2
ccd01ae HEAD@{3}: commit (initial): Commit 1

$ git reset --hard HEAD@{1}        ← wracamy do stanu sprzed pomyłki
HEAD is now at 89a7598 Commit 3
```

`HEAD@{1}` to „tam, gdzie `HEAD` był **jedną zmianę temu**". Odzyskałeś commity.

> `reflog` jest **tylko lokalny** i przechowuje wpisy przez ograniczony czas (domyślnie kilkadziesiąt dni). Ratuje **commity**, a nie
> niezapisane zmiany w plikach.

---

## Powrót do starego commitu „na próbę": `git checkout`

Możesz **obejrzeć** projekt tak, jak wyglądał w dawnym commicie:

```
$ git checkout HEAD~1
HEAD is now at c69b4da Commit 2

$ git status
HEAD detached at c69b4da
nothing to commit, working tree clean
```

**„HEAD detached"** (odłączony `HEAD`) znaczy, że nie jesteś na żadnej gałęzi, tylko „w powietrzu" na wybranym commicie.
Możesz się rozejrzeć, ale **nie rób tu nowych commitów** (nie będą należały do żadnej gałęzi i łatwo je zgubić). Wróć na gałąź:

```
$ git switch main
Previous HEAD position was c69b4da Commit 2
Switched to branch 'main'
```

Git zwykle dodaje długi komunikat z wyjaśnieniem „detached HEAD". To nie błąd, tylko podpowiedź.

---

## Przydatne drobiazgi

| Polecenie | Co robi |
|---|---|
| `git show HEAD~2:plik.txt` | wypisuje **zawartość pliku** z dawnego commitu (bez zmiany czegokolwiek) |
| `git diff HEAD~1` | porównuje katalog roboczy z commitem sprzed jednego kroku |
| `git clean -n` | **podgląd**: które nieśledzone pliki zostałyby usunięte |
| `git clean -f` | ⚠️ **usuwa** nieśledzone pliki z dysku (nieodwracalnie) |

## Ćwiczenia

Ćwiczenie 4 (cofanie zmian) i ćwiczenie 5 (`reset`, `revert`, `reflog`): [cwiczenia.md](cwiczenia.md).

---

⬅️ [3. Pierwsze repozytorium i commity](03-pierwsze-commity.md) · ➡️ [5. Gałęzie i scalanie](05-galezie-i-scalanie.md)
