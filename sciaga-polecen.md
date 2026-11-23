# Ściąga poleceń Git

Wszystkie polecenia z kursu w jednym miejscu. Kolumna „Rozdział" prowadzi do szczegółowego wyjaśnienia.

## Konfiguracja i pomoc

| Polecenie | Co robi | Rozdział |
|---|---|---|
| `git --version` | pokazuje wersję Gita | [2](02-konfiguracja.md) |
| `git config --global user.name "Imię Nazwisko"` | ustawia imię w commitach | [2](02-konfiguracja.md) |
| `git config --global user.email "adres"` | ustawia e-mail w commitach | [2](02-konfiguracja.md) |
| `git config --global init.defaultBranch main` | domyślna nazwa pierwszej gałęzi | [2](02-konfiguracja.md) |
| `git config --global --list` | pokazuje ustawienia | [2](02-konfiguracja.md) |
| `git help <polecenie>` / `git <polecenie> -h` | pomoc | [2](02-konfiguracja.md) |

## Zakładanie i pobieranie repozytorium

| Polecenie | Co robi | Rozdział |
|---|---|---|
| `git init` | zamienia bieżący folder w repozytorium | [3](03-pierwsze-commity.md) |
| `git clone <adres>` | pobiera repozytorium z GitHuba (z historią) | [7](07-zdalne-repozytorium.md) |

## Codzienna praca

| Polecenie | Co robi | Rozdział |
|---|---|---|
| `git status` | pokazuje stan: gałąź, zmiany, poczekalnia | [3](03-pierwsze-commity.md) |
| `git status --short` | to samo w skróconej formie | [3](03-pierwsze-commity.md) |
| `git add <plik>` | dodaje zmiany pliku do poczekalni | [3](03-pierwsze-commity.md) |
| `git add .` | dodaje wszystkie zmiany z bieżącego folderu | [3](03-pierwsze-commity.md) |
| `git commit -m "opis"` | zapisuje poczekalnię w historii | [3](03-pierwsze-commity.md) |
| `git commit -am "opis"` | `add` zmian w śledzonych plikach + `commit` | [3](03-pierwsze-commity.md) |
| `git diff` | zmiany **niedodane** do poczekalni | [3](03-pierwsze-commity.md) |
| `git diff --staged` | zmiany **w poczekalni** (do commitu) | [3](03-pierwsze-commity.md) |
| `git log` / `git log --oneline` | historia commitów (pełna / w jednej linii) | [3](03-pierwsze-commity.md) |
| `git log --graph --oneline --all` | historia z rysunkiem gałęzi | [5](05-galezie-i-scalanie.md) |
| `git show <commit>` | szczegóły jednego commitu | [3](03-pierwsze-commity.md) |
| `git mv stara nowa` | zmiana nazwy lub przeniesienie pliku | [3](03-pierwsze-commity.md) |
| `git rm <plik>` | usunięcie pliku (z dysku i z Gita) | [3](03-pierwsze-commity.md) |
| `git rm --cached <plik>` | przestaje śledzić plik, zostawia go na dysku | [3](03-pierwsze-commity.md) |

## Cofanie i poprawianie

| Polecenie | Co robi | Uwaga |
|---|---|---|
| `git restore <plik>` | cofa zmiany pliku do ostatniego commitu | ⚠️ nieodwracalne |
| `git restore --staged <plik>` | wyjmuje plik z poczekalni | bezpieczne |
| `git restore --source=<commit> <plik>` | przywraca plik z wybranego commitu | |
| `git commit --amend` | poprawia **ostatni** commit | tylko przed `push` |
| `git revert <commit>` | tworzy commit odwracający wskazany | bezpieczne także po `push` |
| `git reset --soft HEAD~1` | cofa commit, zmiany zostają w poczekalni | tylko przed `push` |
| `git reset HEAD~1` | cofa commit, zmiany zostają w plikach | tylko przed `push` |
| `git reset --hard HEAD~1` | cofa commit **i kasuje zmiany** | ⚠️ niebezpieczne |
| `git stash` / `git stash pop` | odkłada zmiany do schowka / przywraca je | |
| `git reflog` | historia ruchów `HEAD` (ratunek po pomyłce) | tylko lokalnie |
| `git clean -n` / `git clean -f` | podgląd / usunięcie nieśledzonych plików | ⚠️ `-f` nieodwracalne |

Szczegóły: [rozdział 4](04-historia-i-cofanie.md).

## Gałęzie i scalanie

| Polecenie | Co robi |
|---|---|
| `git branch` | lista gałęzi (`*` = bieżąca) |
| `git branch <nazwa>` | tworzy gałąź (bez przełączania) |
| `git switch <nazwa>` | przełącza na gałąź |
| `git switch -c <nazwa>` | tworzy gałąź i przełącza |
| `git merge <nazwa>` | wciąga gałąź `<nazwa>` do bieżącej |
| `git merge --abort` | przerywa scalanie z konfliktem |
| `git branch -d <nazwa>` | usuwa scaloną gałąź |
| `git branch -D <nazwa>` | usuwa gałąź **wymuszenie** (⚠️) |
| `git tag <nazwa>` | oznacza bieżący commit etykietą (np. `v1.0`) |

Szczegóły: [rozdział 5](05-galezie-i-scalanie.md) i [6](06-konflikty.md).

## Praca z GitHubem (zdalne repozytorium)

| Polecenie | Co robi |
|---|---|
| `git remote -v` | pokazuje zdalne repozytoria i ich adresy |
| `git remote add origin <adres>` | podpina GitHub pod nazwą `origin` |
| `git push` | wysyła commity na GitHub |
| `git push -u origin <gałąź>` | wysyła i zapamiętuje powiązanie (pierwszy raz dla gałęzi) |
| `git fetch` | pobiera informacje o nowych commitach (nic nie zmienia w plikach) |
| `git pull` | pobiera nowe commity **i włącza** je (`fetch` + `merge`) |
| `git branch -vv` | pokazuje, z którą gałęzią zdalną jest powiązana lokalna |
| `git push origin <tag>` | wysyła tag |

Szczegóły: [rozdział 7](07-zdalne-repozytorium.md).

## Specjalne zapisy

| Zapis | Znaczenie |
|---|---|
| `HEAD` | bieżący commit |
| `HEAD~1`, `HEAD~2` | jeden / dwa commity wcześniej |
| `origin/main` | stan gałęzi `main` na GitHubie (z ostatniego `fetch`) |
| `.` | bieżący folder (np. `git add .`) |
| `-` | poprzednia gałąź (`git switch -`) |
| `.gitignore` | plik z listą ignorowanych plików ([rozdział 9](09-dobre-praktyki.md)) |

## Diagram: dokąd idą zmiany

```
 Katalog roboczy ──git add──▶ Poczekalnia ──git commit──▶ Repozytorium lokalne ──git push──▶ GitHub
        ◀──git restore──              ◀──git restore --staged──     ◀──git pull / git fetch──
```

## Dziesięć poleceń, które wystarczą na początek

```
git status
git add .
git commit -m "opis"
git push
git pull
git log --oneline
git diff
git switch -c nazwa
git merge nazwa
git restore plik
```

---

⬅️ [10. Typowe błędy](10-typowe-bledy.md) · [Spis](README.md)
