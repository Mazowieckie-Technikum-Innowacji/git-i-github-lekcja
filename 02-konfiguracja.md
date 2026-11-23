# 2. Instalacja i konfiguracja

## Terminal: gdzie wpisujemy polecenia

Git obsługujemy poleceniami wpisywanymi w **terminalu** (oknie tekstowym).

- **Windows:** wciśnij klawisz **Windows**, wpisz `PowerShell` lub `Terminal` i otwórz. Albo w folderze w Eksploratorze kliknij
  prawym przyciskiem w pustym miejscu i wybierz **Otwórz w terminalu**.
- **Git Bash:** terminal instalowany razem z Gitem dla Windowsa. Działa tak samo i ma polecenia podobne do Linuksa.
- **Visual Studio Code:** ma terminal wbudowany (menu **Terminal → New Terminal**).

W terminalu zawsze pracujesz **w jakimś folderze**. Przydatne polecenia pomocnicze (nie są częścią Gita):

| Polecenie | Co robi |
|---|---|
| `pwd` (Git Bash) / `Get-Location` (PowerShell) | pokazuje, w jakim folderze jesteś |
| `ls` (obie powłoki) | wypisuje pliki w folderze |
| `cd nazwa-folderu` | wchodzi do folderu |
| `cd ..` | wychodzi o jeden folder wyżej |
| `mkdir nazwa-folderu` | tworzy folder |

## Instalacja Gita

1. Wejdź na **https://git-scm.com/download/win**, pobierz instalator i uruchom go.
2. W instalatorze wystarczy we wszystkich oknach klikać **Next** (ustawienia domyślne są dobre).
3. Po instalacji **zamknij i otwórz terminal na nowo**.

> **GitHub Desktop** (https://desktop.github.com) zawiera własnego Gita, więc jeśli używasz tylko jego, osobna
> instalacja nie jest potrzebna. Polecenia z tego kursu działają w terminalu, więc warto mieć zainstalowany Git.

### Sprawdzenie, czy Git działa

```
$ git --version
git version 2.47.0.windows.1
```

Numer wersji u Ciebie może być inny. Jeśli terminal odpowie, że nie zna polecenia `git`, zamknij go i otwórz ponownie,
a jeśli to nie pomoże, zainstaluj Gita jeszcze raz.

### Pomoc wbudowana w Gita

Git ma pomoc do każdego polecenia:

```
git help commit        # pełna dokumentacja (otwiera się w przeglądarce lub w terminalu)
git commit -h          # krótkie podsumowanie opcji
```

## Konfiguracja: kim jesteś

Każdy commit ma **autora**. Git musi więc wiedzieć, jak się nazywasz i jaki masz adres e-mail. Ustawiasz to **raz na
komputerze**:

```
git config --global user.name "Jan Kowalski"
git config --global user.email "jan.kowalski@example.com"
```

Co robi to polecenie:

| Fragment | Znaczenie |
|---|---|
| `git config` | odczyt i zapis ustawień Gita |
| `--global` | ustawienie obowiązuje **dla wszystkich** Twoich repozytoriów na tym komputerze (zapis w pliku konfiguracyjnym w katalogu domowym) |
| `user.name` | imię i nazwisko, które pojawi się w commitach |
| `user.email` | adres e-mail przypisany do commitów |

> **Który e-mail wpisać?** Tego, którego używasz na GitHubie, bo wtedy GitHub rozpozna Twoje commity jako Twoje.
> Jeśli nie chcesz ujawniać prawdziwego adresu, GitHub oferuje adres typu `ID+login@users.noreply.github.com`
> (znajdziesz go w ustawieniach konta, w sekcji *Emails*).

### Domyślna gałąź

Nowe repozytoria mogą nazywać główną gałąź `master` (starsza nazwa) lub `main` (obecnie standard na GitHubie). Ustaw `main`:

```
git config --global init.defaultBranch main
```

### Sprawdź ustawienia

```
$ git config --global --list
user.name=Jan Kowalski
user.email=jan.kowalski@example.com
init.defaultbranch=main
```

Pojedyncze ustawienie: `git config user.name`.

### Poziomy konfiguracji

| Poziom | Polecenie | Obowiązuje |
|---|---|---|
| systemowy | `git config --system` | dla wszystkich użytkowników komputera |
| **globalny** | `git config --global` | dla Ciebie, we wszystkich repozytoriach |
| lokalny | `git config` (bez opcji, w repozytorium) | tylko w tym jednym repozytorium |

Lokalne ustawienie **wygrywa** z globalnym. Przydaje się, gdy w jednym projekcie chcesz używać innego e-maila.

## Ćwiczenie 1 (około 5 minut)

Zainstaluj Gita (jeśli go nie ma), ustaw imię, nazwisko i e-mail i sprawdź wynik poleceniem
`git config --global --list`. Szczegóły i kryteria: [cwiczenia.md](cwiczenia.md).

---

⬅️ [1. Czym są Git i GitHub](01-wprowadzenie.md) · ➡️ [3. Pierwsze repozytorium i commity](03-pierwsze-commity.md)
