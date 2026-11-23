# 1. Czym są Git i GitHub

## Problem, który rozwiązuje Git

Wyobraź sobie, że piszesz program albo wypracowanie. Z czasem pojawiają się foldery w stylu:

```
projekt
projekt_final
projekt_final_v2
projekt_final_v2_NAPRAWDE_OSTATECZNY
```

Takie podejście ma trzy wady:

1. **Nie wiadomo, co się zmieniło** między wersjami.
2. **Nie da się łatwo wrócić** do wersji sprzed dwóch dni, gdy coś się zepsuło.
3. **Praca w kilka osób** kończy się nadpisywaniem cudzych plików.

**Git** rozwiązuje to wszystko: zapamiętuje **kolejne wersje projektu** razem z opisem, kto i co zmienił, i pozwala do
każdej z nich wrócić.

## Git i GitHub to nie to samo

| | Git | GitHub |
|---|---|---|
| Co to jest | **program** na Twoim komputerze | **strona internetowa** (usługa w chmurze) |
| Do czego | zapisuje historię zmian w projekcie | przechowuje kopie repozytoriów w internecie, pozwala współpracować |
| Czy działa bez internetu | **tak** | nie |
| Kto go stworzył | Linus Torvalds (2005, do pracy nad jądrem Linuksa) | firma GitHub (dziś należy do Microsoftu) |

Prosta analogia: **Git to aparat** (robi zdjęcia kolejnych wersji), a **GitHub to album w chmurze**, do którego wysyłasz zdjęcia,
żeby inni mogli je zobaczyć i dodawać swoje.

Istnieją też inne „albumy": GitLab, Bitbucket. Git działa z każdym z nich tak samo.

## Najważniejsze pojęcia

| Pojęcie | Wyjaśnienie |
|---|---|
| **repozytorium** (*repository*, repo) | folder projektu **razem z całą jego historią**. Historia leży w ukrytym folderze `.git` |
| **commit** | jeden zapisany stan projektu z opisem („punkt zapisu" w grze). Ma unikalny identyfikator (skrót) |
| **historia** (*history*) | ciąg commitów, od najstarszego do najnowszego |
| **gałąź** (*branch*) | osobna linia pracy; pozwala testować zmiany bez ruszania głównej wersji |
| **`main`** | domyślna, główna gałąź projektu |
| **zdalne repozytorium** (*remote*) | kopia repozytorium w internecie (np. na GitHubie) |
| **`origin`** | domyślna nazwa zdalnego repozytorium, z którego sklonowałeś projekt |
| **klon** (*clone*) | kopia całego repozytorium (z historią) na Twoim komputerze |
| **push** | wysłanie commitów z komputera na GitHub |
| **pull** | pobranie nowych commitów z GitHuba i włączenie ich do Twojej pracy |

## Trzy obszary Gita (to najważniejsza rzecz w tym kursie)

Git pracuje z plikami w **trzech miejscach**. Gdy to zrozumiesz, polecenia przestają być magią.

```
 Katalog roboczy        Poczekalnia             Repozytorium
 (working directory)    (staging area / index)  (historia commitów)
 ┌──────────────┐       ┌──────────────┐        ┌──────────────┐
 │ edytujesz    │ add   │ wybierasz,   │ commit │ zapisane     │
 │ pliki tutaj  │ ────▶ │ co wejdzie   │ ─────▶ │ wersje       │
 │              │       │ do commitu   │        │              │
 └──────────────┘       └──────────────┘        └──────────────┘
```

1. **Katalog roboczy:** zwykłe pliki w folderze projektu. Tu piszesz kod.
2. **Poczekalnia** (*staging area*): lista zmian, które **zamierzasz** zapisać w najbliższym commicie.
3. **Repozytorium:** zapisana historia. Commit „zamraża" to, co jest w poczekalni.

Dlaczego jest poczekalnia? Bo możesz zmienić pięć plików, a zapisać tylko dwa (te, które łączy jedna sprawa). Dzięki
temu historia jest czytelna: **jeden commit to jedna logiczna zmiana**.

Każdy plik w projekcie jest w jednym z czterech stanów:

| Stan | Co znaczy | Jak go zobaczysz w `git status` |
|---|---|---|
| **Nieśledzony** (*untracked*) | Git jeszcze nie zna tego pliku | `Untracked files` |
| **Zmieniony** (*modified*) | znany plik, w którym coś zmieniłeś | `Changes not staged for commit` |
| **W poczekalni** (*staged*) | zmiana oznaczona do zapisania | `Changes to be committed` |
| **Zapisany** (*committed*) | zmiana jest w historii | nie pojawia się w statusie |

## Jak Git „widzi" Twój projekt

- Commit nie zapisuje „różnic", tylko **cały stan projektu** (w sprytny, oszczędny sposób). Dlatego do każdej wersji można
  wrócić natychmiast.
- Każdy commit ma **identyfikator** (skrót SHA-1, np. `6aeaf5f`), **autora**, **datę**, **opis** i wskazanie na **poprzedni
  commit**. Dzięki temu commity tworzą łańcuch (historię).
- **Gałąź to tylko etykieta** wskazująca na jeden z commitów. To dlatego tworzenie gałęzi jest w Gicie błyskawiczne.
- **`HEAD`** to „jesteś tutaj": wskazuje, który commit (zwykle przez gałąź) jest teraz wczytany w Twoim folderze.

## Pytania do przemyślenia

1. Czym różni się Git od GitHuba?
2. Po co Git ma poczekalnię, skoro mógłby od razu zapisywać wszystkie zmiany?
3. Co dzieje się z historią projektu, gdy wyślesz go na GitHub: czy GitHub dostaje tylko ostatnią wersję?

*(Odpowiedzi znajdziesz w pliku [pytania-kontrolne.md](pytania-kontrolne.md).)*

---

⬅️ [Spis](README.md) · ➡️ [2. Instalacja i konfiguracja](02-konfiguracja.md)
