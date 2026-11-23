# 3. Pierwsze repozytorium i commity

W tym rozdziale zrobisz pełny obieg pracy z Gitem na swoim komputerze: **utworzyć repozytorium → zmienić plik → dodać do
poczekalni → zapisać commit → obejrzeć historię**.

Wszystko robimy w osobnym folderze do ćwiczeń, np. `cwiczenia-git`.

```
mkdir cwiczenia-git
cd cwiczenia-git
mkdir projekt
cd projekt
```

---

## `git init`: założenie repozytorium

```
$ git init
Initialized empty Git repository in .../projekt/.git/
```

**Co robi:** zamienia **bieżący folder** w repozytorium Gita.

**Pod spodem:** tworzy ukryty folder **`.git`**. To w nim Git trzyma całą historię projektu. Twoje pliki leżą obok, w zwykłych
folderach. Usunięcie folderu `.git` oznacza utratę historii (pliki zostają, ale projekt nie jest już repozytorium).

**Uwaga:** wykonuj `git init` **tylko raz na projekt** i tylko w folderze projektu. Nie rób go w folderze domowym ani na
całym dysku, bo Git zacznie śledzić wszystko, co tam jest.

---

## `git status`: co się dzieje w projekcie

To **najczęściej używane polecenie Gita**. Pokazuje, na której jesteś gałęzi i co jest zmienione.

```
$ git status
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

| Fragment | Znaczenie |
|---|---|
| `On branch main` | jesteś na gałęzi `main` |
| `No commits yet` | w historii nie ma jeszcze żadnego commitu |
| `nothing to commit` | nie ma żadnych zmian do zapisania |

**Zasada:** nie wiesz, co się dzieje? Wpisz `git status`. Robisz to przed każdym `add` i `commit`.

Skrócona wersja: `git status --short` (albo `-s`) wypisuje po jednej linii na plik:

| Znaczek | Znaczenie |
|---|---|
| `??` | nieśledzony plik |
| ` M` (spacja, M) | zmieniony, **niezapisany w poczekalni** |
| `M ` (M, spacja) | zmieniony i **w poczekalni** |
| `A ` | nowy plik w poczekalni |
| `R ` | zmieniona nazwa |
| `D ` | usunięty |

---

## Tworzymy plik

Utwórz plik `hello.txt` z tekstem „Hello" (np. w Notatniku, albo w terminalu: `echo Hello > hello.txt`).

```
$ git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	hello.txt

nothing added to commit but untracked files present (use "git add" to track)
```

Plik jest **nieśledzony** (*untracked*): leży w folderze, ale Git go jeszcze nie zna. Sam wskazuje następny krok:
`git add`.

---

## `git add`: dodanie do poczekalni

```
$ git add hello.txt
$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   hello.txt
```

**Co robi:** **kopiuje aktualną zawartość pliku do poczekalni**, czyli oznacza go jako „wejdzie do następnego commitu".

| Wariant | Co dodaje |
|---|---|
| `git add plik.txt` | jeden wskazany plik |
| `git add plik1.txt plik2.txt` | kilka wskazanych plików |
| `git add src/` | wszystkie pliki z folderu `src` |
| `git add .` | wszystkie zmiany w **bieżącym folderze i podfolderach** (nowe, zmienione, usunięte) |
| `git add -A` | wszystkie zmiany w **całym repozytorium** |
| `git add -p` | interaktywnie: Git pyta o każdy fragment zmiany (zaawansowane, bardzo przydatne) |

**Ważne:** `git add` zapisuje stan pliku **z chwili wywołania**. Jeśli po `add` jeszcze coś zmienisz w pliku, ta nowa zmiana
**nie** jest w poczekalni (musisz zrobić `add` ponownie).

> **Pułapka:** `git add .` dodaje **wszystko**, także pliki, których nie chcesz (np. foldery `bin/`, `obj/`, hasła w pliku
> `.env`). Dlatego przed `commit` zawsze zrób `git status`. Jak się przed tym chronić: [rozdział 9](09-dobre-praktyki.md).

---

## `git commit`: zapisanie w historii

```
$ git commit -m "Pierwszy commit"
[main (root-commit) 219f6ab] Pierwszy commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```

**Co robi:** tworzy **commit**, czyli zapisuje w historii wszystko, co jest w poczekalni, razem z opisem, autorem i datą.

| Fragment wyniku | Znaczenie |
|---|---|
| `main` | gałąź, na której zapisano commit |
| `(root-commit)` | to **pierwszy** commit w repozytorium (nie ma poprzednika) |
| `219f6ab` | **skrót** (identyfikator) commitu |
| `1 file changed, 1 insertion(+)` | podsumowanie: 1 plik, 1 dodana linia |
| `create mode 100644` | utworzono nowy zwykły plik (kod `100644` oznacza plik bez uprawnień do uruchamiania) |

| Opcja | Co robi |
|---|---|
| `-m "opis"` | opis commitu podany od razu w poleceniu |
| (bez `-m`) | Git otworzy edytor tekstu, w którym wpisujesz opis (pierwsza linia = tytuł) |
| `-a` | przed commitem automatycznie dodaje do poczekalni **zmiany w plikach już śledzonych** (nie dodaje nowych plików!). Skrót: `git commit -am "opis"` |
| `--amend` | poprawia **ostatni** commit (patrz [rozdział 4](04-historia-i-cofanie.md)) |

> Jeśli Git otworzył edytor (zwykle **Vim**) i nie wiesz, jak z niego wyjść: wciśnij `Esc`, wpisz `:wq` i `Enter`
> (zapisz i wyjdź). Aby wyjść **bez** zapisywania: `Esc`, `:q!`, `Enter`.

**Dobry opis commitu** odpowiada na pytanie „co ta zmiana robi?", np. „Dodano walidację liczby", a nie „zmiany" czy „asdf".
Więcej w [rozdziale 9](09-dobre-praktyki.md).

---

## `git log`: historia

```
$ git log
commit 219f6ab81cde01502768349a0cf2c4782d442cd2
Author: Jan Kowalski <jan@example.com>
Date:   Tue Nov 24 10:00:00 2026 +0100

    Pierwszy commit
```

**Co robi:** wypisuje historię od **najnowszego** commitu do najstarszego.

| Pole | Znaczenie |
|---|---|
| `commit 219f6ab81c…` | pełny identyfikator (40 znaków). Zwykle wystarcza pierwsze 7 |
| `Author` | autor (z konfiguracji `user.name` i `user.email`) |
| `Date` | kiedy zapisano commit |
| reszta | opis commitu |

Przydatne opcje:

| Polecenie | Co pokazuje |
|---|---|
| `git log --oneline` | każdy commit w **jednej linii** (skrót + opis) |
| `git log -3` | tylko 3 ostatnie commity |
| `git log --stat` | pliki zmienione w każdym commicie |
| `git log --graph --oneline --all` | **graf** gałęzi i scaleń ([rozdział 5](05-galezie-i-scalanie.md)) |
| `git log --author="Jan"` | commity wskazanego autora |
| `git log -- plik.txt` | commity, które zmieniały dany plik |

Jeśli lista jest długa, Git pokaże ją w trybie przewijania: strzałkami przewijasz, **`q`** kończy.

---

## Druga zmiana i `git diff`

Zmień plik `hello.txt` na „Hello, Git!" i zapytaj Gita, **co dokładnie** się zmieniło:

```
$ git diff
diff --git a/hello.txt b/hello.txt
index e965047..670a245 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1 @@
-Hello
+Hello, Git!
```

**Jak to czytać:**

| Fragment | Znaczenie |
|---|---|
| `--- a/hello.txt` / `+++ b/hello.txt` | porównujemy **starą** (`a`) i **nową** (`b`) wersję pliku |
| `@@ -1 +1 @@` | zmieniona jest linia 1 (stara wersja) i linia 1 (nowa wersja) |
| linia z `-` (czerwona) | **usunięta** linia |
| linia z `+` (zielona) | **dodana** linia |

Dwa warianty `git diff`:

| Polecenie | Porównuje |
|---|---|
| `git diff` | katalog roboczy z **poczekalnią** (zmiany, których jeszcze **nie dodałeś**) |
| `git diff --staged` (lub `--cached`) | poczekalnię z **ostatnim commitem** (zmiany **zaplanowane do commitu**) |

Po `git add hello.txt` zwykłe `git diff` jest puste, a zmianę zobaczysz przez `git diff --staged`:

```
$ git add hello.txt
$ git diff --staged
diff --git a/hello.txt b/hello.txt
index e965047..670a245 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1 @@
-Hello
+Hello, Git!
```

Zapisz zmianę:

```
$ git commit -m "Zmiana powitania"
[main 6aeaf5f] Zmiana powitania
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git log --oneline
6aeaf5f Zmiana powitania
219f6ab Pierwszy commit
```

> **Nawyk dobrego programisty:** przed każdym commitem uruchom `git status` i `git diff --staged`. Sprawdzisz, że zapisujesz
> dokładnie to, co chcesz.

---

## `git show`: szczegóły jednego commitu

```
$ git show --stat HEAD
commit 6aeaf5fcaff4dd4c147f3fd70da4a49e8bf827b4
Author: Jan Kowalski <jan@example.com>
Date:   Tue Nov 24 10:00:00 2026 +0100

    Zmiana powitania

 hello.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

**Co robi:** pokazuje opis i zmiany wskazanego commitu. Bez `--stat` zobaczysz pełny `diff`.

| Zapis | Znaczenie |
|---|---|
| `HEAD` | bieżący (najnowszy) commit |
| `HEAD~1` | commit **o jeden wcześniejszy** od `HEAD` |
| `HEAD~2` | o dwa wcześniejszy |
| `6aeaf5f` | konkretny commit (skrót) |

---

## Pliki, które chcesz przenieść lub usunąć

Zwykłe przenoszenie czy usuwanie pliku w Eksploratorze też zadziała (Git zauważy zmianę), ale Git ma własne polecenia, które
od razu wrzucają zmianę do poczekalni:

| Polecenie | Co robi |
|---|---|
| `git mv stara.txt nowa.txt` | zmienia nazwę (lub przenosi) pliku **i** dodaje zmianę do poczekalni |
| `git rm plik.txt` | usuwa plik z dysku **i** dodaje usunięcie do poczekalni |
| `git rm --cached plik.txt` | przestaje śledzić plik, ale **zostawia go na dysku** (przydatne, gdy dodałeś go przez pomyłkę) |

```
$ git mv plik.txt notatki.txt
$ git status --short
R  plik.txt -> notatki.txt
```

---

## Podsumowanie obiegu pracy

```
edytujesz pliki  →  git status  →  git add  →  git commit  →  (powtarzasz)
```

| Krok | Polecenie |
|---|---|
| Co się zmieniło? | `git status`, `git diff` |
| Wybierz, co zapisać | `git add plik` |
| Zapisz | `git commit -m "opis"` |
| Zobacz historię | `git log --oneline` |

## Ćwiczenia

Ćwiczenie 2 (pierwsze repozytorium i dwa commity) i ćwiczenie 3 (praca na kilku plikach): [cwiczenia.md](cwiczenia.md).

---

⬅️ [2. Instalacja i konfiguracja](02-konfiguracja.md) · ➡️ [4. Historia i cofanie zmian](04-historia-i-cofanie.md)
