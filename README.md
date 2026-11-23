# Git i GitHub: lekcja krok po kroku

Kurs dla uczniów, którzy zaczynają z **Gitem** i **GitHubem**. Każde polecenie jest wyjaśnione dokładnie: **co robi,
co dzieje się „pod spodem", jak wygląda jego wynik i co może pójść nie tak**. Wszystkie przykłady uruchomiono naprawdę,
a wyniki w ramkach pochodzą z prawdziwych uruchomień (numery commitów u Ciebie będą inne, to normalne).

> Materiał przygotowany w Mazowieckim Technikum Innowacji w Płocku. Na lekcjach pracujemy na GitHubie i w Classroom 50,
> więc cały kurs prowadzi do umiejętności, które są potrzebne przy zadaniach programistycznych.

## Dla kogo i czego się nauczysz

Po tym kursie potrafisz:

- założyć repozytorium, zapisywać zmiany (commity) i czytać historię projektu,
- cofać błędy w bezpieczny sposób,
- pracować na gałęziach i scalać je,
- rozwiązywać konflikty,
- wysyłać pracę na GitHub (`push`) i pobierać zmiany (`pull`, `fetch`),
- korzystać z GitHuba: pull requesty, zgłoszenia (issues), fork, GitHub Actions,
- rozumieć komunikaty błędów Gita i wiedzieć, co z nimi zrobić.

**Nie musisz** znać programowania. Wystarczy umieć otworzyć terminal i napisać plik tekstowy.

## Spis rozdziałów

| # | Rozdział | Co w nim jest | Czas |
|---|---|---|---|
| 1 | [Czym są Git i GitHub](01-wprowadzenie.md) | po co są, jak działają, trzy „obszary" Gita | 15 min |
| 2 | [Instalacja i konfiguracja](02-konfiguracja.md) | `git --version`, `git config`, terminal | 10 min |
| 3 | [Pierwsze repozytorium i commity](03-pierwsze-commity.md) | `init`, `status`, `add`, `commit`, `log`, `diff`, `show` | 30 min |
| 4 | [Historia i cofanie zmian](04-historia-i-cofanie.md) | `restore`, `reset`, `revert`, `amend`, `stash`, `tag`, `reflog` | 30 min |
| 5 | [Gałęzie i scalanie](05-galezie-i-scalanie.md) | `branch`, `switch`, `merge`, `log --graph` | 25 min |
| 6 | [Konflikty](06-konflikty.md) | skąd się biorą i jak je rozwiązać | 20 min |
| 7 | [Zdalne repozytorium](07-zdalne-repozytorium.md) | `remote`, `clone`, `push`, `pull`, `fetch` | 30 min |
| 8 | [GitHub w praktyce](08-github-w-praktyce.md) | pull request, issues, fork, Actions, Classroom 50 | 30 min |
| 9 | [Dobre praktyki](09-dobre-praktyki.md) | opisy commitów, `.gitignore`, sekrety | 15 min |
| 10 | [Typowe błędy i jak je naprawić](10-typowe-bledy.md) | komunikaty błędów po polsku | do czytania |
| | [Ściąga poleceń](sciaga-polecen.md) | wszystkie polecenia w jednej tabeli | |
| | [Ćwiczenia](cwiczenia.md) | 8 ćwiczeń z kryteriami sprawdzenia | |
| | [Pytania kontrolne](pytania-kontrolne.md) | pytania z odpowiedziami do samokontroli | |
| | [Plan zajęć](plan-zajec.md) | propozycja przebiegu dwóch lekcji | |

## Jak korzystać

1. **Czytaj po kolei**, rozdziały budują się na poprzednich.
2. **Powtarzaj przykłady w terminalu.** Najlepiej w osobnym folderze do ćwiczeń (nie w projekcie z zadania).
3. Po każdym rozdziale zrób **ćwiczenie** z pliku [cwiczenia.md](cwiczenia.md).
4. Gdy pojawi się komunikat błędu, zajrzyj do [rozdziału 10](10-typowe-bledy.md).
5. Kiedy zapomnisz polecenie: [ściąga](sciaga-polecen.md).

## Jak czytać przykłady

W przykładach znak `$` oznacza **polecenie, które Ty wpisujesz** (znaku `$` nie wpisuj). Pod nim jest **wynik**:

```
$ git status
On branch main
nothing to commit, working tree clean
```

- Wyniki w kursie są po **angielsku**. Git na Twoim komputerze może wypisywać je po polsku. Znaczenie jest takie samo.
- **Skróty commitów** (np. `219f6ab`) to losowe znaki. U Ciebie będą inne.
- Tam, gdzie polecenie jest **niebezpieczne** (np. kasuje zmiany bez możliwości cofnięcia), jest wyraźne ostrzeżenie.

## Co jeszcze przyda się w szkole

- [Słowniczek](https://mazowieckie-technikum-innowacji.github.io/materialy-programowanie/slowniczek.html) pojęć programistycznych
- [Classroom 50 dla uczniów](https://mazowieckie-technikum-innowacji.github.io/materialy-programowanie/classroom50-dla-uczniow.html)
- [GitHub Desktop](https://mazowieckie-technikum-innowacji.github.io/materialy-programowanie/github-desktop.html): Git w okienkach

## Licencja

Treści: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.pl). Możesz je kopiować i przerabiać,
pod warunkiem podania źródła i udostępnienia przeróbek na tej samej licencji.
