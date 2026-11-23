# Ćwiczenia

Osiem ćwiczeń do wykonania **po kolei**, w miarę czytania kursu. Każde ma **kroki** i **kryterium sprawdzenia**: wynik, który
musisz zobaczyć w terminalu. Jeśli wynik jest inny, wróć do kroków albo zajrzyj do [rozdziału 10](10-typowe-bledy.md).

> Ćwiczenia wykonuj w **osobnym folderze do nauki** (np. `cwiczenia-git`), a **nie** w repozytorium z zadania ocenianego przez
> Classroom 50. Ćwiczenie 8 dotyczy GitHuba i ma własne repozytorium.

---

## Ćwiczenie 1: konfiguracja (rozdział 2)

1. Sprawdź, czy Git jest zainstalowany: `git --version`.
2. Ustaw swoje imię, nazwisko i e-mail (`git config --global user.name ...`, `git config --global user.email ...`).
3. Ustaw domyślną gałąź: `git config --global init.defaultBranch main`.
4. Wypisz ustawienia: `git config --global --list`.

**Sprawdzenie:** wynik `git config --global --list` zawiera linie `user.name=...`, `user.email=...` i `init.defaultbranch=main`.

---

## Ćwiczenie 2: pierwsze repozytorium (rozdział 3)

1. Utwórz folder `projekt` i wejdź do niego. Wykonaj `git init`.
2. Utwórz plik `hello.txt` z tekstem `Hello`.
3. Wykonaj `git status`. Co jest napisane o pliku `hello.txt`?
4. Dodaj plik do poczekalni (`git add hello.txt`) i sprawdź `git status` jeszcze raz.
5. Zapisz commit: `git commit -m "Pierwszy commit"`.
6. Zmień treść pliku na `Hello, Git!`. Wykonaj `git diff`.
7. Dodaj zmianę i zapisz drugi commit: `Zmiana powitania`.

**Sprawdzenie:** `git log --oneline` pokazuje **dwa** commity z Twoimi opisami, a `git status` wypisuje
`nothing to commit, working tree clean`.

**Pytanie dodatkowe:** czym różni się wynik `git diff` od `git diff --staged` w kroku 6, zanim i po tym jak zrobisz `git add`?

---

## Ćwiczenie 3: kilka plików i `.gitignore` (rozdział 3 i 9)

1. W swoim repozytorium utwórz pliki `a.txt` i `b.txt` oraz folder `bin` z plikiem `bin/smiec.dll`.
2. Wykonaj `git status --short`. Co oznacza `??`?
3. Utwórz plik `.gitignore` z jedną linią `bin/`. Wykonaj `git status --short` jeszcze raz: folder `bin` powinien zniknąć z listy.
4. Dodaj **tylko** `a.txt` i `.gitignore` (`git add a.txt .gitignore`) i zapisz commit „Dodano a i gitignore".
5. Dodaj `b.txt` w **osobnym** commicie „Dodano b".

**Sprawdzenie:** `git log --oneline` pokazuje dwa nowe commity, a `git status` jest czysty (folder `bin` jest ignorowany).

---

## Ćwiczenie 4: cofanie zmian (rozdział 4)

1. Dopisz do `a.txt` linię `blad`. Wykonaj `git status --short` (powinno być ` M a.txt`).
2. **Cofnij** tę zmianę poleceniem `git restore a.txt`. Sprawdź zawartość pliku.
3. Utwórz plik `c.txt`, dodaj go do poczekalni, a potem **wyjmij z poczekalni** (`git restore --staged c.txt`).
4. Zapisz commit z plikiem `c.txt`, ale z **literówką** w opisie (np. „Dodano c.txt z literufką").
5. Popraw opis ostatniego commitu: `git commit --amend -m "Dodano c.txt"`.

**Sprawdzenie:** `git log --oneline` pokazuje poprawny opis, a plik `a.txt` nie zawiera linii `blad`.

---

## Ćwiczenie 5: `reset`, `revert`, `reflog` (rozdział 4)

Pracuj na repozytorium z czterema commitami (dodaj commit, jeśli masz mniej).

1. Zapisz w pliku `a.txt` nową linię i zrób commit „Zly commit".
2. **Wycofaj go bezpiecznie** przez `git revert --no-edit HEAD`. Sprawdź `git log --oneline` (zobaczysz commit `Revert "Zly commit"`).
3. Teraz zrób celowo „awarię": `git reset --hard HEAD~2`. Sprawdź `git log --oneline` (cofnąłeś dwa commity).
4. **Odzyskaj je:** wypisz `git reflog`, znajdź wpis sprzed `reset`, i wykonaj `git reset --hard HEAD@{1}`.

**Sprawdzenie:** po kroku 4 `git log --oneline` pokazuje te same commity co po kroku 2.

> ⚠️ `reset --hard` kasuje niezapisane zmiany. Wykonuj ten krok tylko wtedy, gdy `git status` jest czysty.

---

## Ćwiczenie 6: gałąź i scalenie (rozdział 5)

1. Utwórz gałąź i przełącz się: `git switch -c nowa-funkcja`.
2. Utwórz plik `funkcja.txt`, dodaj go i zapisz **dwa** commity (np. dopisz linię w drugim).
3. Wróć na `main` (`git switch main`) i sprawdź, że pliku `funkcja.txt` **nie ma**.
4. Scal gałąź: `git merge nowa-funkcja`. Zwróć uwagę na słowo `Fast-forward`.
5. Obejrzyj historię: `git log --oneline --graph --all`.
6. Usuń gałąź: `git branch -d nowa-funkcja`.

**Sprawdzenie:** `funkcja.txt` jest na `main`, a `git branch` pokazuje tylko `main`.

**Rozszerzenie:** powtórz ćwiczenie, ale **przed scaleniem** zrób jeszcze jeden commit na `main` (osobny plik). Co dzieje się teraz
z `git merge`? (Powstaje commit scalający zamiast `Fast-forward`.)

---

## Ćwiczenie 7: konflikt (rozdział 6)

1. Utwórz plik `wspolny.txt` z linią `Wspolna` i zapisz commit.
2. Utwórz gałąź `b1`, w której zmienisz zawartość pliku na `Wersja z b1`. Zapisz commit.
3. Wróć na `main` i zmień **ten sam** plik na `Wersja z main`. Zapisz commit.
4. Wykonaj `git merge b1`. Co mówi Git?
5. Otwórz `wspolny.txt`. Rozpoznaj znaczniki `<<<<<<<`, `=======`, `>>>>>>>`.
6. Rozwiąż konflikt: zostaw jedną linię `Wersja po rozwiazaniu`, bez znaczników.
7. `git add wspolny.txt`, a potem `git commit --no-edit`.

**Sprawdzenie:** `git log --oneline --graph` pokazuje commit scalający (`Merge branch 'b1'`), a w pliku nie ma żadnych znaczników.

**Dla chętnych:** powtórz ćwiczenie i w kroku 4 użyj potem `git merge --abort`. Co się stało ze stanem repozytorium?

---

## Ćwiczenie 8: GitHub (rozdział 7 i 8)

1. Na GitHubie, **na swoim koncie osobistym** (nie w organizacji szkoły), utwórz **nowe, puste prywatne repozytorium** `moje-cwiczenia` (przycisk **New**). **Nie** zaznaczaj opcji dodania README.
2. W folderze `projekt` z poprzednich ćwiczeń podepnij GitHub:
   `git remote add origin https://github.com/TWOJ-LOGIN/moje-cwiczenia.git` i sprawdź `git remote -v`.
3. Wyślij repozytorium: `git push -u origin main`. (Zaloguj się w oknie przeglądarki, jeśli o to poprosi.)
4. Odśwież stronę repozytorium na GitHubie: czy widzisz swoje pliki i historię commitów?
5. **Zmień plik na GitHubie** (kliknij plik → ikona ołówka → zmiana → **Commit changes**).
6. W terminalu wykonaj `git fetch` i `git status`. Co mówi o `origin/main`?
7. Pobierz zmianę: `git pull`.
8. Zrób lokalnie nową zmianę, `commit` i `git push`.

**Sprawdzenie:** na GitHubie widać wszystkie commity (zrobione lokalnie i w przeglądarce), a `git status` mówi
`Your branch is up to date with 'origin/main'`.

**Dla chętnych (pull request):**
1. Utwórz gałąź `poprawka`, zrób zmianę i commit, wyślij: `git push -u origin poprawka`.
2. Na GitHubie kliknij **Compare & pull request**, nadaj tytuł i utwórz PR.
3. W zakładce **Files changed** zobacz porównanie. Scal PR (**Merge pull request**) i usuń gałąź.
4. U siebie: `git switch main`, `git pull`.

---

## Zadanie końcowe (przed klasówką lub dla chętnych)

Bez zaglądania do kursu:

1. Utwórz repozytorium lokalnie, zrób trzy commity z sensownymi opisami.
2. Utwórz gałąź, zrób na niej commit i scal ją do `main`.
3. Podepnij repozytorium do GitHuba i wyślij.
4. Wywołaj **celowy konflikt** (zmień ten sam plik w dwóch miejscach: na GitHubie i lokalnie) i rozwiąż go.

Pokaż nauczycielowi wynik `git log --oneline --graph --all`.

---

⬅️ [Spis](README.md) · ➡️ [Pytania kontrolne](pytania-kontrolne.md)
