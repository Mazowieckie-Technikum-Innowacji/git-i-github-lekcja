# 7. Zdalne repozytorium: `clone`, `push`, `pull`, `fetch`

Dotąd pracowałeś **tylko u siebie**. Teraz podłączymy projekt do **GitHuba** (zdalnego repozytorium), żeby mieć kopię w
chmurze, pracować na kilku komputerach i współpracować z innymi.

## Obraz całości

```
    Twój komputer                                  GitHub (origin)
 ┌───────────────────┐   git push  ──────────▶  ┌───────────────────┐
 │ lokalne           │                          │ zdalne            │
 │ repozytorium      │   git fetch ◀──────────  │ repozytorium      │
 │ (z historią)      │   git pull  ◀──────────  │ (z historią)      │
 └───────────────────┘                          └───────────────────┘
        git clone: pobranie całego repozytorium z GitHuba na komputer (raz, na początku)
```

- **`git clone`**: robisz **raz**, żeby pobrać projekt z GitHuba.
- **`git push`**: wysyłasz **swoje nowe commity** na GitHub.
- **`git fetch`**: pobierasz **informacje** o nowych commitach z GitHuba (nic nie zmieniasz w plikach).
- **`git pull`**: pobierasz nowe commity **i od razu włączasz je do swojej pracy** (to `fetch` + `merge`).

---

## Logowanie do GitHuba z poziomu Gita

Od 2021 roku GitHub **nie przyjmuje hasła konta** przy operacjach Gita (`clone`, `push`, `pull`). Są dwa wygodne sposoby:

1. **Git Credential Manager** (instalowany razem z Gitem dla Windowsa). Przy pierwszym `push` lub `clone` prywatnego repozytorium
   otworzy się **okno przeglądarki** z prośbą o zalogowanie do GitHuba. Po zalogowaniu dane są zapamiętane.
2. **Token dostępu osobistego** (*personal access token*) wklejany zamiast hasła. Bardziej skomplikowane i mniej bezpieczne,
   dlatego **zwykle wystarczy opcja 1**.

Adres repozytorium można zapisać na dwa sposoby:

| Rodzaj | Przykład | Uwagi |
|---|---|---|
| **HTTPS** | `https://github.com/organizacja/repozytorium.git` | najprostszy, polecany na początek |
| **SSH** | `git@github.com:organizacja/repozytorium.git` | wymaga skonfigurowania klucza SSH, wygodny na stałe |

Adres skopiujesz na stronie repozytorium przyciskiem zielonym **Code**.

---

## `git clone`: pobranie repozytorium

```
$ git clone https://github.com/organizacja/repozytorium.git
Cloning into 'repozytorium'...
done.
```

**Co robi:**
1. tworzy nowy folder o nazwie repozytorium,
2. pobiera **całą historię** projektu,
3. rozpakowuje pliki (z domyślnej gałęzi, zwykle `main`),
4. **zapisuje adres GitHuba pod nazwą `origin`**, żeby później wiedzieć, dokąd wysyłać.

Potem wchodzisz do folderu: `cd repozytorium`.

| Polecenie | Co robi |
|---|---|
| `git clone <adres>` | klonuje do folderu o nazwie repozytorium |
| `git clone <adres> mój-folder` | klonuje do folderu o wskazanej nazwie |
| `git clone -b gałąź <adres>` | po sklonowaniu przełącza na wskazaną gałąź |

> **Klonowanie wykonujesz raz.** Potem pracujesz w tym folderze i synchronizujesz przez `push` i `pull`. Jeśli sklonujesz to
> samo repozytorium drugi raz, dostaniesz drugi, niezależny folder.

> W zadaniach z Classroom 50 **klonujesz własne repozytorium** (z Twoim loginem w nazwie), a nie szablon zadania.

---

## `git remote`: zdalne repozytoria

Gdy tworzysz repozytorium **najpierw lokalnie** (`git init`), musisz je **podpiąć do GitHuba**. Najpierw utwórz **puste**
repozytorium na GitHubie (przycisk **New**), a potem:

```
$ git remote add origin https://github.com/organizacja/repozytorium.git
$ git remote -v
origin	https://github.com/organizacja/repozytorium.git (fetch)
origin	https://github.com/organizacja/repozytorium.git (push)
```

| Polecenie | Co robi |
|---|---|
| `git remote add origin <adres>` | **dodaje** zdalne repozytorium i nadaje mu nazwę `origin` |
| `git remote -v` | **wypisuje** zdalne repozytoria i ich adresy (osobno do pobierania i wysyłania) |
| `git remote set-url origin <nowy-adres>` | zmienia adres (np. po przeniesieniu repozytorium) |
| `git remote remove origin` | odłącza zdalne repozytorium |

**`origin`** to tylko **umowna nazwa** (skrót zamiast długiego adresu). Mógłbyś nazwać je inaczej, ale wszyscy używają `origin`.

---

## `git push`: wysłanie commitów na GitHub

Pierwszy raz dla gałęzi:

```
$ git push -u origin main
To https://github.com/organizacja/repozytorium.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

| Fragment | Znaczenie |
|---|---|
| `git push` | wyślij commity |
| `origin` | dokąd (nazwa zdalnego repozytorium) |
| `main` | którą gałąź |
| `-u` (`--set-upstream`) | **zapamiętaj** powiązanie lokalnej gałęzi `main` z `origin/main`, żeby kolejne `git push` i `git pull` działały **bez podawania argumentów** |
| `[new branch]` | na GitHubie gałąź powstała (wcześniej jej tam nie było) |

Następne razy wystarczy:

```
git push
```

**Co robi `push`:** wysyła na GitHub **te commity, których tam jeszcze nie ma**, i przesuwa zdalną gałąź do przodu.
Nie wysyła niezapisanych zmian: najpierw musi być `commit`.

### Nowa gałąź nie ma jeszcze powiązania

```
$ git switch -c moja
$ git push
fatal: The current branch moja has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin moja
```

To **nie** awaria: Git mówi, co zrobić. Wykonaj `git push -u origin moja` (tylko za pierwszym razem).

```
$ git push -u origin moja
To ../serwer.git
 * [new branch]      moja -> moja
branch 'moja' set up to track 'origin/moja'.
```

### Co oznacza wynik zwykłego `push`

```
$ git push
To https://github.com/organizacja/repozytorium.git
   1aeff70..7c1694f  main -> main
```

`1aeff70..7c1694f` to zakres wysłanych commitów (od ostatnio znanego na GitHubie do najnowszego Twojego).

### `push` odrzucony

```
$ git push
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/organizacja/repozytorium.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
```

**Co to znaczy:** na GitHubie są commity, **których Ty nie masz u siebie** (ktoś je wysłał wcześniej, albo Ty z innego
komputera, albo Classroom 50 dodał commit). Git **nie pozwala nadpisać** cudzej pracy.

**Co zrobić:** najpierw `git pull`, rozwiąż ewentualny konflikt, a potem `git push`.

> **Nigdy** nie rozwiązuj tego przez `git push --force`. To nadpisuje zdalną historię i **kasuje cudzą pracę**.
> Na wspólnych gałęziach (`main`) GitHub zwykle i tak to blokuje.

---

## `git fetch`: pobierz informacje

```
$ git fetch
From https://github.com/organizacja/repozytorium
   1aeff70..cba8664  main       -> origin/main
```

**Co robi:** **pobiera** nowe commity z GitHuba, ale **niczego nie zmienia** w Twoich plikach ani w Twojej gałęzi `main`.
Zapisuje je w „lustrzanej" gałęzi **`origin/main`** (stan GitHuba z chwili ostatniego `fetch`).

Potem możesz sprawdzić, jak daleko jesteś od GitHuba:

```
$ git status
On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```

I zobaczyć, co przyszło:

```
$ git log --oneline --all
1aeff70 Pierwszy commit
cba8664 Dodano plik kolegi
```

`git fetch` jest **bezpieczny**: nic nie psuje. Dobrze go używać, żeby „zajrzeć" na GitHub przed pracą.

Możliwe komunikaty w `git status`:

| Komunikat | Co znaczy | Co zrobić |
|---|---|---|
| `Your branch is up to date with 'origin/main'` | jesteś zsynchronizowany | nic |
| `Your branch is ahead of 'origin/main' by N commits` | masz **N commitów do wysłania** | `git push` |
| `Your branch is behind 'origin/main' by N commits` | na GitHubie są **nowe commity** | `git pull` |
| `Your branch and 'origin/main' have diverged` | **obie strony** mają własne commity | `git pull` (scalenie), potem `git push` |

---

## `git pull`: pobierz i włącz

```
$ git pull
Updating 1aeff70..cba8664
Fast-forward
 kolega.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 kolega.txt
```

**Co robi:** to skrót dwóch poleceń: **`git fetch` + `git merge origin/main`**. Pobiera zmiany i od razu włącza je do bieżącej gałęzi.

**Kiedy `pull` jest bezproblemowy:** gdy Ty nie masz nowych commitów (tak jak wyżej, wynik `Fast-forward`).

**Kiedy robi się ciekawie:** gdy **obie strony** mają nowe commity (gałęzie się „rozeszły"). Nowsze wersje Gita pytają wtedy,
**jak je połączyć**, i przerywają:

```
$ git pull
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint:
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
...
fatal: Need to specify how to reconcile divergent branches.
```

**To nie błąd**, tylko pytanie. Na początek wybierz **scalanie** (merge) jednym poleceniem, **raz**:

```
git config --global pull.rebase false
```

i powtórz `git pull`. Gdy zmiany dotyczą różnych miejsc, Git scali je sam (powstanie commit scalający), a gdy tego samego
fragmentu, wystąpi **konflikt** ([rozdział 6](06-konflikty.md)).

> Opcja `pull --rebase` (przekładanie własnych commitów na wierzch cudzych) daje „prostszą" historię, ale jest trudniejsza
> przy konfliktach i wymaga większej wprawy. Zostaw ją na później.

### Dobra rutyna pracy

```
git pull          ← na początku pracy: pobierz cudze zmiany
(pracujesz, robisz commity)
git push          ← na końcu: wyślij swoją pracę
```

---

## Podsumowanie: które polecenie kiedy

| Chcę… | Polecenie |
|---|---|
| pobrać projekt z GitHuba po raz pierwszy | `git clone <adres>` |
| podpiąć lokalny projekt do GitHuba | `git remote add origin <adres>` |
| wysłać swoje commity | `git push` (pierwszy raz: `git push -u origin <gałąź>`) |
| sprawdzić, czy na GitHubie coś nowego, nic nie zmieniając | `git fetch` + `git status` |
| pobrać nowe commity i włączyć | `git pull` |
| zobaczyć, dokąd wysyłam | `git remote -v` |
| zobaczyć powiązanie gałęzi | `git branch -vv` |

## Ćwiczenie

Ćwiczenie 8 (repozytorium na GitHubie: `clone`, `commit`, `push`, `pull`): [cwiczenia.md](cwiczenia.md).

---

⬅️ [6. Konflikty](06-konflikty.md) · ➡️ [8. GitHub w praktyce](08-github-w-praktyce.md)
