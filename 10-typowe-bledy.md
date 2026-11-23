# 10. Typowe błędy i jak je naprawić

Komunikaty Gita są **po angielsku**, ale zwykle mówią wprost, co jest nie tak i co zrobić. Naucz się je **czytać**: ostatnie
linie (`hint:`) często podają gotowe rozwiązanie. Poniżej najczęstsze sytuacje z rozwiązaniami.

> Komunikaty w ramkach niżej pochodzą z prawdziwych uruchomień. Gdzie komunikat jest typowy, ale go nie odtwarzaliśmy
> w przykładzie, jest to zaznaczone słowem *(typowy)*.

---

## 1. `fatal: not a git repository`

```
$ git status
fatal: not a git repository (or any of the parent directories): .git
```

**Znaczy:** jesteś w folderze, który **nie jest repozytorium** (nie ma w nim ani w folderach nadrzędnych folderu `.git`).

**Co zrobić:**
- sprawdź, w którym folderze jesteś (`pwd` lub `Get-Location`) i **przejdź do folderu projektu** (`cd nazwa`),
- jeśli to nowy projekt: `git init`,
- jeśli chciałeś pobrać projekt z GitHuba: `git clone <adres>`.

---

## 2. Git nie wie, kim jesteś

Na komputerach, gdzie nie ustawiono imienia i e-maila, Git albo **ustawi dane automatycznie na podstawie nazwy użytkownika i komputera**
(i wypisze ostrzeżenie, że to mogą nie być Twoje dane), albo przerwie z komunikatem w stylu *(typowy)*:

```
Author identity unknown
*** Please tell me who you are.
```

**Co zrobić:** ustaw swoje dane ([rozdział 2](02-konfiguracja.md)):

```
git config --global user.name "Imię Nazwisko"
git config --global user.email "twoj-mail@example.com"
```

Jeśli commit już powstał z błędnym autorem, popraw go (tylko jeśli nie był wysłany): `git commit --amend --reset-author`.

---

## 3. `nothing to commit, working tree clean`

```
$ git commit -m "Nic nowego"
On branch main
nothing to commit, working tree clean
```

**Znaczy:** nie ma żadnych zmian do zapisania. To **nie błąd**: albo już wszystko zapisałeś, albo zapomniałeś zapisać plik w edytorze.

**Co zrobić:** sprawdź, czy zapisałeś plik (`Ctrl+S`) i czy jesteś w dobrym folderze.

Podobne: `no changes added to commit`: masz zmiany, ale **nie dodałeś ich do poczekalni**. Zrób `git add`.

---

## 4. `pathspec ... did not match any files`

```
$ git add nieistniejacy.txt
fatal: pathspec 'nieistniejacy.txt' did not match any files
```

**Znaczy:** plik o takiej nazwie nie istnieje. **Co zrobić:** sprawdź nazwę (`ls`), wielkość liter i rozszerzenie
(np. `plik.txt` kontra `plik.txt.txt`, bo Windows potrafi ukryć rozszerzenia).

---

## 5. `push` odrzucony: `rejected ... fetch first`

```
$ git push
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to '...'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
```

**Znaczy:** na GitHubie są commity, których nie masz u siebie. **Co zrobić:** `git pull`, ewentualnie rozwiąż konflikt
([rozdział 6](06-konflikty.md)), potem `git push`. **Nie używaj `--force`.**

W repozytoriach z zadań Classroom 50 zdarza się to po tym, gdy **nauczyciel lub Classroom 50 dopisał commit** w Twoim repozytorium
(np. zmiana README). Zawsze `git pull` przed pracą.

---

## 6. `Need to specify how to reconcile divergent branches`

```
hint: You have divergent branches and need to specify how to reconcile them.
...
fatal: Need to specify how to reconcile divergent branches.
```

**Znaczy:** Ty i GitHub macie **różne nowe commity** i Git pyta, jak je połączyć. **Co zrobić:** wybierz scalanie (raz) i powtórz `pull`:

```
git config --global pull.rebase false
git pull
```

---

## 7. `The current branch ... has no upstream branch`

```
$ git push
fatal: The current branch moja has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin moja
```

**Znaczy:** nowa gałąź nie jest jeszcze powiązana ze zdalną. **Co zrobić:** `git push -u origin moja` (jak sugeruje komunikat).

---

## 8. Konflikt scalania: `CONFLICT (content)`

```
CONFLICT (content): Merge conflict in wspolny.txt
Automatic merge failed; fix conflicts and then commit the result.
```

**Co zrobić:** otwórz plik, usuń znaczniki `<<<<<<<`, `=======`, `>>>>>>>`, zostaw wersję docelową, `git add plik`, `git commit`.
Gdy się zgubisz: `git merge --abort`. Szczegóły: [rozdział 6](06-konflikty.md).

---

## 9. Zmiany blokują przełączenie gałęzi

```
$ git switch galaz
error: Your local changes to the following files would be overwritten by checkout:
	a.txt
Please commit your changes or stash them before you switch branches.
Aborting
```

**Znaczy:** masz niezapisane zmiany w pliku, który różni się na docelowej gałęzi, i przełączenie by je **nadpisało**.

**Co zrobić:** zapisz zmiany (`git add` + `git commit`) **albo** odłóż je (`git stash`), przełącz gałąź, a potem (jeśli odłożyłeś)
`git stash pop`. Nie wykonuj `git restore`, jeśli zmiany są potrzebne.

---

## 10. `remote origin already exists`

```
$ git remote add origin https://github.com/organizacja/y.git
error: remote origin already exists.
```

**Znaczy:** `origin` jest już ustawiony (np. po `git clone`). **Co zrobić:** sprawdź `git remote -v`. Jeśli adres jest zły, zmień go:
`git remote set-url origin <poprawny-adres>`.

---

## 11. `'origin' does not appear to be a git repository`

```
$ git push origin main
fatal: 'origin' does not appear to be a git repository
fatal: Could not read from remote repository.
```

**Znaczy:** w tym repozytorium **nie ma zdalnego repozytorium o nazwie `origin`** (np. zrobiłeś `git init` i nie podpiąłeś GitHuba).
**Co zrobić:** `git remote add origin <adres>` ([rozdział 7](07-zdalne-repozytorium.md)).

---

## 12. `src refspec main does not match any` *(typowy)*

```
error: src refspec main does not match any
error: failed to push some refs to '...'
```

**Znaczy:** próbujesz wysłać gałąź `main`, **która jeszcze nie ma żadnego commitu** (albo nazywa się inaczej, np. `master`).
**Co zrobić:** zrób pierwszy commit, a nazwę gałęzi sprawdź przez `git branch`.

---

## 13. `Repository not found` / `403` / `Authentication failed` *(typowe)*

```
remote: Repository not found.
fatal: repository 'https://github.com/organizacja/repozytorium.git/' not found
```

albo `fatal: Authentication failed` / `remote: Permission to ... denied` / błąd `403`.

**Znaczy:** albo adres jest zły, albo **nie masz dostępu** (Twoje konto nie ma uprawnień do tego repozytorium), albo jesteś zalogowany
na **inne konto**. GitHub dla prywatnych repozytoriów, do których nie masz dostępu, udaje, że ich **nie ma** (stąd „not found").

**Co zrobić:**
- sprawdź **adres** (`git remote -v`) i literówki,
- sprawdź, czy jesteś zalogowany na **właściwe konto** (w Git Credential Manager albo w przeglądarce),
- sprawdź, czy przyjąłeś zaproszenie do organizacji i czy masz dostęp do repozytorium,
- w zadaniach z Classroom 50 pracuj w **swoim** repozytorium (z Twoim loginem), a nie w szablonie.

---

## 14. `refusing to merge unrelated histories` *(typowy)*

```
fatal: refusing to merge unrelated histories
```

**Znaczy:** próbujesz połączyć dwa repozytoria, które **nie mają wspólnej historii** (np. lokalny `git init` i osobno utworzone repozytorium
na GitHubie z README). **Co zrobić:** najlepiej **sklonuj** repozytorium z GitHuba i skopiuj do niego pliki. Jeśli wiesz, co robisz:
`git pull origin main --allow-unrelated-histories`.

---

## 15. `cannot pull with rebase: You have unstaged changes` *(typowy)*

**Znaczy:** masz niezapisane zmiany, a `pull` skonfigurowany jest w trybie `rebase`. **Co zrobić:** zapisz zmiany (`commit`) albo odłóż (`stash`),
a potem `git pull`.

---

## 16. `HEAD detached at ...`

```
$ git status
HEAD detached at c69b4da
```

**Znaczy:** przełączyłeś się na konkretny commit, a nie na gałąź. **Co zrobić:** jeśli tylko oglądałeś, wróć: `git switch main`. Jeśli zrobiłeś tu
nowe commity i chcesz je zachować: `git switch -c nowa-galaz` (utworzy gałąź z Twoją pracą).

---

## 17. Ostrzeżenie o końcach linii: `LF will be replaced by CRLF`

```
warning: in the working copy of 'plik.txt', LF will be replaced by CRLF the next time Git touches it
```

**Znaczy:** Windows i Linux zapisują koniec linii inaczej (`CRLF` kontra `LF`). Git na Windowsie to **automatycznie ujednolica**.
**Co zrobić:** zwykle **nic**. To ostrzeżenie, a nie błąd.

---

## 18. Commit na złej gałęzi

Zrobiłeś commit na `main`, a chciałeś na osobnej gałęzi (jeszcze **bez `push`**). Napraw to:

```
git branch nowa-galaz          # utwórz gałąź z bieżącym commitem
git reset --hard HEAD~1        # cofnij commit z main (⚠️ tylko gdy to jeden commit i nic więcej nie masz niezapisane)
git switch nowa-galaz          # przejdź do nowej gałęzi: commit tam jest
```

Jeśli nie jesteś pewien, zapytaj nauczyciela, zanim użyjesz `reset --hard`.

---

## 19. Wysłałeś do repozytorium coś, czego nie chciałeś (np. hasło)

- **Hasło lub klucz:** **natychmiast je zmień** (unieważnij). Usunięcie pliku z repozytorium nie wystarczy ([rozdział 9](09-dobre-praktyki.md)).
- **Zbędne pliki (`bin`, `obj`):** dopisz do `.gitignore`, a z repozytorium usuń: `git rm -r --cached bin obj`, potem `commit` i `push`.
- **Błędny commit:** `git revert <commit>` i `push`.

---

## 20. Nie wiem, co się stało

1. **Nie panikuj.** Prawie wszystko w Gicie da się odzyskać.
2. Wpisz **`git status`** i przeczytaj, co mówi.
3. Wpisz **`git log --oneline --graph --all`**, żeby zobaczyć historię.
4. Wpisz **`git reflog`**, żeby zobaczyć, co robił `HEAD` ([rozdział 4](04-historia-i-cofanie.md)).
5. **Zrób zrzut ekranu** komunikatu i **zapytaj nauczyciela**. Najlepiej pokaż wynik `git status` i `git log --oneline -5`.
6. **Nie kasuj całego folderu** i nie klonuj od nowa „na szybko", dopóki nie sprawdzisz, czy da się odzyskać pracę.

---

⬅️ [9. Dobre praktyki](09-dobre-praktyki.md) · ➡️ [Ściąga poleceń](sciaga-polecen.md)
