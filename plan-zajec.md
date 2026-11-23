# Plan zajęć: Git i GitHub (propozycja)

Propozycja przebiegu **dwóch lekcji po 90 minut** (albo czterech po 45). Czasy są orientacyjne. Dostosuj tempo do grupy.
Każdy uczeń pracuje **na własnym komputerze**, w terminalu, i powtarza polecenia razem z prowadzącym.

## Przed zajęciami: lista kontrolna

- [ ] Na komputerach w pracowni zainstalowany **Git** (`git --version`) i **GitHub Desktop** (opcjonalnie).
- [ ] Uczniowie mają **konta GitHub** i są dodani do organizacji szkoły (zaakceptowane zaproszenia).
- [ ] Terminal, w którym będziecie pracować, jest wybrany (PowerShell / Git Bash / terminal w VS Code) i **taki sam dla wszystkich**.
- [ ] Prowadzący ma przygotowane **repozytorium pokazowe** (np. w folderze `demo`) i pusty folder do ćwiczeń.
- [ ] Zdecydowane, czy uczniowie pracują **przez terminal**, czy częściowo **w GitHub Desktop** (wtedy dodaj czas na rozdział z GitHub Desktop).

## Lekcja 1 (90 min): praca lokalna

| Czas | Temat | Co robimy | Materiał |
|---|---|---|---|
| 0-10 | Po co Git? | problem „projekt_final_v2", pokaz historii prawdziwego projektu na GitHubie | [rozdz. 1](01-wprowadzenie.md) |
| 10-20 | Trzy obszary Gita | rysunek: katalog roboczy → poczekalnia → repozytorium. **Uczniowie zapisują go w zeszycie** | [rozdz. 1](01-wprowadzenie.md) |
| 20-30 | Instalacja i konfiguracja | `git --version`, `git config`. **Ćwiczenie 1** | [rozdz. 2](02-konfiguracja.md) |
| 30-60 | Pierwsze repozytorium | `init`, `status`, `add`, `commit`, `log`, `diff`. **Ćwiczenia 2 i 3**. Prowadzący robi, uczniowie powtarzają | [rozdz. 3](03-pierwsze-commity.md) |
| 60-85 | Cofanie zmian | `restore`, `amend`, `revert`, `reset`, `reflog`. **Ćwiczenia 4 i 5**. Wyraźne ostrzeżenie o `reset --hard` | [rozdz. 4](04-historia-i-cofanie.md) |
| 85-90 | Podsumowanie | 3 pytania kontrolne ustnie | [pytania](pytania-kontrolne.md) |

**Na co uważać:**
- Uczniowie **zapominają o `git add`** i pytają, czemu `commit` nic nie zapisał. Wracaj do rysunku z trzema obszarami.
- Zdarza się, że Git otwiera **edytor Vim** przy `commit` bez `-m`. Pokaż wyjście (`Esc`, `:wq`).
- Uczniowie mogą mieć **różne wersje** terminala i Gita (różny wygląd komunikatów). To normalne.

## Lekcja 2 (90 min): gałęzie, GitHub, współpraca

| Czas | Temat | Co robimy | Materiał |
|---|---|---|---|
| 0-5 | Powtórka | 3 pytania z lekcji 1 | [pytania](pytania-kontrolne.md) |
| 5-30 | Gałęzie i scalanie | `branch`, `switch`, `merge`, `log --graph`. **Ćwiczenie 6** | [rozdz. 5](05-galezie-i-scalanie.md) |
| 30-50 | Konflikty | celowy konflikt w parach lub samodzielnie. **Ćwiczenie 7** | [rozdz. 6](06-konflikty.md) |
| 50-75 | Zdalne repozytorium | `remote add`, `push -u`, `fetch`, `pull`, `clone`. **Ćwiczenie 8** | [rozdz. 7](07-zdalne-repozytorium.md) |
| 75-88 | GitHub w praktyce | pull request (pokaz), issues, Actions. Przejście do **zadań w Classroom 50** | [rozdz. 8](08-github-w-praktyce.md) |
| 88-90 | Zadanie domowe | patrz niżej | |

**Pomysł na wspólne ćwiczenie (pary):**
1. Uczeń A tworzy repozytorium na GitHubie i dodaje partnera jako współpracownika (*Settings → Collaborators*).
2. Obaj klonują repozytorium. Każdy zmienia **ten sam plik** i próbuje wypchnąć. Drugi dostaje `rejected` i musi rozwiązać konflikt.
3. Wynik oglądają razem w zakładce **Commits** na GitHubie.

> W organizacji szkolnej uczniowie mogą mieć ograniczone prawa tworzenia repozytoriów i zapraszania osób spoza szkoły.
> Wspólne ćwiczenie w parach najwygodniej zrobić w **repozytorium utworzonym przez nauczyciela** albo **w prywatnych kontach uczniów**.

## Zadanie domowe

1. Przeczytaj [rozdział 9](09-dobre-praktyki.md) i [rozdział 10](10-typowe-bledy.md).
2. Wykonaj [zadanie końcowe](cwiczenia.md) (z końca pliku z ćwiczeniami).
3. Przyjmij w Classroom 50 zadanie „Git: pierwszy commit" (jeśli jeszcze nie przyjęte) i zapamiętaj, które polecenia z kursu w nim wykorzystujesz.

## Ocenianie (propozycja)

| Element | Jak sprawdzić |
|---|---|
| Ćwiczenia 2, 6, 8 | wynik `git log --oneline --graph --all` pokazany nauczycielowi (lub zrzut ekranu) |
| Ćwiczenie 7 | commit scalający w historii, brak znaczników konfliktu w pliku |
| Pytania kontrolne | krótki sprawdzian ustny lub pisemny (np. 10 z 27 pytań) |
| Zadanie w Classroom 50 | automatyczne testy (np. „Git: pierwszy commit") |

## Najczęstsze trudności uczniów (z doświadczenia)

| Trudność | Co pomaga |
|---|---|
| „Nic nie zapisało się w commicie" | `git status`: czy plik jest w poczekalni (`git add`)? |
| Pomylenie `git add` z `git commit` | rysunek trzech obszarów, powtórzony kilka razy |
| Lęk przed `reset --hard` | najpierw pokaż `reflog` jako siatkę bezpieczeństwa, dopiero potem `reset --hard` |
| Konflikt „zepsuł" plik | `git merge --abort` i spokojne wyjaśnienie znaczników |
| Praca w niewłaściwym folderze | `pwd`, `git status`: czy w ogóle jesteśmy w repozytorium? |
| Pomylenie GitHuba z Gitem | analogia: aparat (Git) i album w chmurze (GitHub) |
| Pomylenie szablonu z własnym repozytorium w Classroom 50 | [rozdział 8](08-github-w-praktyce.md) i [przewodnik Classroom 50](https://mazowieckie-technikum-innowacji.github.io/materialy-programowanie/classroom50-dla-uczniow.html) |

---

⬅️ [Spis](README.md)
