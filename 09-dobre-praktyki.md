# 9. Dobre praktyki

Git pozwala na wszystko, ale **dobre nawyki** sprawiają, że historia projektu jest czytelna, a praca wygodna.

## Opisy commitów

Opis commitu to wiadomość dla przyszłego Ciebie i innych: **co ta zmiana robi**.

**Zasady:**

1. Pierwsza linia: **krótki tytuł** (do ok. 50-70 znaków), w trybie rozkazującym lub opisowym: „Dodano walidację liczby".
2. Opisuj **co i po co**, a nie „jak" (jak widać w kodzie).
3. **Jeden commit = jedna logiczna zmiana.** Nie wrzucaj do jednego commitu naprawy błędu, nowej funkcji i zmiany formatowania.
4. Jeśli trzeba, po pustej linii dopisz dłuższe wyjaśnienie (w edytorze, bez `-m`).

| ❌ Słabo | ✅ Dobrze |
|---|---|
| `zmiany` | `Dodano obsługę dzielenia przez zero` |
| `asdf` | `Poprawiono literówkę w README` |
| `Poprawki` | `Zmieniono nazwę klasy Notatka na Note` |
| `Dzisiejsza praca` | `Dodano konstruktor jednoargumentowy` |

## Jak często robić commity

- **Często i w małych kawałkach.** Po każdej działającej, spójnej zmianie. Łatwiej cofnąć jeden mały commit niż wielki.
- **Nie zostawiaj pracy niezapisanej** na koniec dnia. Commit i `push` to także kopia zapasowa.
- **Przed commitem:** `git status` i `git diff --staged`, żeby sprawdzić, co zapisujesz.
- **Nie commituj niedziałającego kodu** na `main` (na własnej gałęzi możesz).

## Plik `.gitignore`: czego nie zapisywać

Nie każdy plik powinien trafić do repozytorium. **Nie wrzucamy:**

- plików **generowanych automatycznie** (foldery `bin/`, `obj/`, `node_modules/`, pliki `.exe`),
- plików **z ustawieniami Twojego komputera** (np. folder `.vs/`),
- plików **z hasłami i kluczami** (`.env`, pliki z tokenami),
- dużych plików (filmy, instalatory).

**`.gitignore`** to plik tekstowy z **listą wzorców**, które Git ma ignorować:

```
bin/
obj/
.vs/
*.user
.env
```

| Wzorzec | Znaczenie |
|---|---|
| `bin/` | ignoruj folder `bin` (i wszystko w nim) |
| `*.user` | ignoruj wszystkie pliki z końcówką `.user` |
| `.env` | ignoruj plik o tej nazwie |
| `!ważny.txt` | **wyjątek**: nie ignoruj tego pliku |
| `#` na początku linii | komentarz |

Efekt:

```
$ git status --short
?? .env
?? bin/

(tworzysz plik .gitignore z dwiema liniami: bin/ oraz .env)

$ git status --short
?? .gitignore               ← bin/ i .env zniknęły z listy, bo są w .gitignore
```

**Gotowy plik dla projektów .NET:** w folderze projektu wpisz `dotnet new gitignore`. GitHub ma też gotowe szablony przy
tworzeniu repozytorium (pole **Add .gitignore**).

> **`.gitignore` działa tylko na pliki, których Git jeszcze nie śledzi.** Jeśli plik **już jest w repozytorium**, samo dopisanie go
> do `.gitignore` nie pomoże. Musisz go „odpiąć": `git rm --cached plik` (plik zostaje na dysku) i zrobić commit.

## Sekrety: hasła, klucze, tokeny

**Nigdy nie wrzucaj do repozytorium haseł, kluczy API ani tokenów**, szczególnie do **publicznego**. Boty skanują GitHuba
w poszukiwaniu takich danych w ciągu **minut**.

**Co robić, gdy wyślesz sekret przez pomyłkę:**

1. **Natychmiast zmień (unieważnij) hasło lub klucz** w usłudze, której dotyczył. To jest najważniejsze.
2. Dopiero potem usuń go z repozytorium. **Samo usunięcie pliku nie wystarczy**, bo sekret zostaje w **historii** commitów.
3. Czyszczenie historii jest trudne (wymaga narzędzi do przepisywania historii). Dlatego tak ważne jest, żeby **zmienić sekret**.

**Profilaktyka:** trzymaj sekrety w plikach z `.gitignore` (np. `.env`) albo w zmiennych środowiskowych, a w repozytorium dodaj
**przykładowy** plik bez prawdziwych wartości (np. `.env.example`).

## Kolejność pracy w dniu

```
1. git pull                       ← pobierz zmiany
2. git switch -c zadanie-x        ← (opcjonalnie) osobna gałąź
3. pracujesz, rób małe commity    ← git add, git commit
4. git push                       ← wyślij
```

## Częste złe nawyki

| Zły nawyk | Dlaczego źle | Co zamiast tego |
|---|---|---|
| `git add .` bez sprawdzenia | wrzucisz śmieci i sekrety | `git status` przed `add`, `.gitignore` |
| Jeden wielki commit na koniec tygodnia | nie da się niczego cofnąć ani zrozumieć | małe, częste commity |
| `git push --force` „żeby działało" | kasuje cudzą pracę | `git pull`, rozwiąż konflikt, `git push` |
| Praca bezpośrednio na `main` w zespole | trudno cofać błędy | gałąź + pull request |
| Opisy „zmiany", „fix", „asdf" | historia jest bezużyteczna | opis mówi, co robi zmiana |
| Kopiowanie folderu `.git` między projektami | miesza historie | `git clone` lub `git init` |

## Zasady bezpieczeństwa

- **Nie ufaj ślepo poleceniom z internetu.** Nie wklejaj do terminala komend, których nie rozumiesz, szczególnie z
  `--force`, `reset --hard`, `clean -f`, `rm -rf`.
- **Trzymaj w repozytoriach publicznych tylko to, co może zobaczyć każdy.** W szkole: bez danych uczniów, haseł i rozwiązań
  bieżących zadań.
- **Pracuj samodzielnie, gdy zadanie tego wymaga.** Commity mają autora i datę, więc łatwo zobaczyć, kto co napisał.

---

⬅️ [8. GitHub w praktyce](08-github-w-praktyce.md) · ➡️ [10. Typowe błędy i jak je naprawić](10-typowe-bledy.md)
