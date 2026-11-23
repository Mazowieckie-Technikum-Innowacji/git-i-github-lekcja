# 5. Gałęzie i scalanie

## Po co są gałęzie

Wyobraź sobie, że:

- **projekt działa** i jest zapisany na gałęzi `main`,
- chcesz dodać nową funkcję, ale **nie wiesz, czy się uda**,
- nie chcesz psuć działającej wersji.

**Gałąź** (*branch*) to **osobna linia pracy**. Tworzysz ją, eksperymentujesz, a gdy wszystko działa, **scalasz** (*merge*)
z `main`. Jeśli nie wyjdzie, wyrzucasz gałąź i `main` pozostaje nietknięty.

> **W Gicie gałąź to tylko etykieta (wskaźnik) na jednym commicie.** Dlatego tworzenie i przełączanie gałęzi trwa ułamek
> sekundy i nic nie kopiuje.

```
        A───B───C  ← main
                 \
                  D───E  ← nowa-funkcja
```

`HEAD` wskazuje gałąź, na której aktualnie pracujesz. Każdy nowy commit przesuwa **tę** gałąź do przodu.

---

## `git branch`: lista i tworzenie gałęzi

```
$ git branch
* main
```

**Co robi:** wypisuje gałęzie lokalne. Gwiazdka `*` oznacza **bieżącą**.

| Polecenie | Co robi |
|---|---|
| `git branch` | lista gałęzi |
| `git branch -vv` | lista ze skrótem ostatniego commitu i gałęzią zdalną |
| `git branch nazwa` | **tworzy** gałąź (ale **nie przełącza** się na nią) |
| `git branch -d nazwa` | usuwa gałąź, **jeśli jest już scalona** (bezpieczne) |
| `git branch -D nazwa` | usuwa gałąź **bez sprawdzania** (⚠️ możesz stracić niescalone commity) |
| `git branch -m nowa-nazwa` | zmienia nazwę bieżącej gałęzi |

**Nazwy gałęzi:** krótkie, bez spacji i polskich znaków, najlepiej opisowe: `nowa-funkcja`, `poprawka-logowania`,
`zadanie-kalkulator`.

---

## `git switch`: przełączanie

```
$ git switch -c nowa-funkcja
Switched to a new branch 'nowa-funkcja'
```

| Polecenie | Co robi |
|---|---|
| `git switch nazwa` | przełącza na **istniejącą** gałąź |
| `git switch -c nazwa` | **tworzy** gałąź i od razu się na nią przełącza (`-c` = *create*) |
| `git switch -` | wraca na poprzednią gałąź |

> **Starszy zapis:** `git checkout nazwa` i `git checkout -b nazwa` robią to samo (dostępne wszędzie, ale `checkout` ma też
> inne zadania, więc łatwo go pomylić). `git switch` jest od Gita 2.23.

**Co się dzieje z plikami po przełączeniu?** Git **podmienia pliki w folderze** tak, by odpowiadały gałęzi, na którą się
przełączasz. Jeśli masz niezapisane zmiany, które by kolidowały, Git odmówi przełączenia i poprosi o `commit` lub `stash`.

Sprawdź to sam:

```
$ git switch -c nowa-funkcja
$ echo "z galezi" > funkcja.txt
$ git add funkcja.txt
$ git commit -m "Dodano funkcje"
[nowa-funkcja 2126f07] Dodano funkcje
 1 file changed, 1 insertion(+)
 create mode 100644 funkcja.txt

$ git branch
  main
* nowa-funkcja

$ git switch main
Switched to branch 'main'
$ ls
hello.txt  notatki.txt         ← pliku funkcja.txt tu NIE ma, bo leży na innej gałęzi
```

Wróć na `nowa-funkcja`, a `funkcja.txt` pojawi się z powrotem. Każda gałąź ma swój zestaw plików.

---

## `git merge`: scalanie

Gdy funkcja jest gotowa, **wracasz na gałąź docelową** (zwykle `main`) i scalasz do niej gałąź z pracą:

```
$ git switch main
$ git merge nowa-funkcja
Updating 08fe28b..2126f07
Fast-forward
 funkcja.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 funkcja.txt
```

**Zapamiętaj kierunek:** `git merge X` **wciąga gałąź `X` do tej, na której jesteś teraz**.

### Dwa rodzaje scalenia

**1. Fast-forward** (przewinięcie), jak wyżej. `main` **nie miał nowych commitów** od momentu utworzenia gałęzi, więc Git po
prostu **przesuwa etykietę `main` do przodu**. Nie powstaje nowy commit.

```
przed:   A──B──C  ← main            po:   A──B──C──D──E  ← main
                \                                       ← nowa-funkcja
                 D──E  ← nowa-funkcja
```

**2. Commit scalający** (*merge commit*). Obie gałęzie **mają własne, różne commity**. Git tworzy **nowy commit z dwoma
rodzicami**, łączący obie linie.

```
$ git switch -c a
$ echo A > a.txt; git add a.txt; git commit -m "Dodano a"
$ git switch main
$ echo M > m.txt; git add m.txt; git commit -m "Dodano m"
$ git merge --no-edit a
Merge made by the 'ort' strategy.
 a.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 a.txt
```

### Opcje `merge`

| Opcja | Znaczenie |
|---|---|
| `--no-edit` | użyj domyślnego opisu commitu scalającego, bez otwierania edytora |
| `--no-ff` | **zawsze** twórz commit scalający (nawet gdy dałoby się przewinąć) |
| `--abort` | **przerwij** scalanie, które zatrzymało się z konfliktem, i wróć do stanu sprzed `merge` |

---

## `git log --graph`: zobacz gałęzie na rysunku

```
$ git log --oneline --graph
*   e9baece Merge branch 'a'
|\
| * f8c5dd3 Dodano a
* | ac02560 Dodano m
|/
* 2126f07 Dodano funkcje
* 08fe28b Dopisano druga linie
* 542acd0 Zly commit
```

**Jak czytać rysunek:**

- Każda gwiazdka `*` to commit. **Pionowa linia `|`** to gałąź.
- `|\` i `|/` pokazują **rozejście się i spotkanie** gałęzi.
- Commit `e9baece` na górze ma **dwóch rodziców**: `f8c5dd3` i `ac02560`. To commit scalający.

Dodaj `--all`, żeby zobaczyć **wszystkie** gałęzie (a nie tylko bieżącą).

---

## Sprzątanie po scaleniu

Gdy gałąź jest scalona, możesz ją usunąć (same commity zostają w historii):

```
$ git branch -d nowa-funkcja
Deleted branch nowa-funkcja (was 2126f07).
```

Gdyby gałąź **nie była scalona**, `-d` odmówi: Git chroni Cię przed utratą pracy. Dopiero `-D` wymusi usunięcie.

---

## Jak pracować z gałęziami w praktyce

1. **`main` ma być zawsze działający.** Nie pracujesz na nim bezpośrednio przy większych zmianach.
2. Do **każdego zadania** (funkcji, poprawki) tworzysz osobną gałąź.
3. Gdy skończysz, scalasz gałąź do `main` (najczęściej przez **pull request** na GitHubie, [rozdział 8](08-github-w-praktyce.md)).
4. Scaloną gałąź usuwasz.

> W zadaniach z Classroom 50 zwykle pracujesz bezpośrednio na `main` (jedna osoba, jedno repozytorium), a ocenianie
> uruchamia się po każdym pushu na `main`. Gałęzie przydadzą się w projektach zespołowych i przy własnych eksperymentach.

## Ćwiczenie

Ćwiczenie 6 (gałąź, dwa commity, scalenie): [cwiczenia.md](cwiczenia.md).

---

⬅️ [4. Historia i cofanie zmian](04-historia-i-cofanie.md) · ➡️ [6. Konflikty](06-konflikty.md)
