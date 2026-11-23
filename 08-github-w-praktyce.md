# 8. GitHub w praktyce

Git działa na Twoim komputerze. **GitHub** dodaje do niego **miejsce w internecie** i narzędzia do współpracy:
repozytoria, zgłoszenia, pull requesty, automatyczne testy. W tym rozdziale przejdziemy po tym, czego będziesz używać
najczęściej.

## Konto i bezpieczeństwo

- Załóż konto na **https://github.com** (albo użyj konta szkolnego, które dostałeś od nauczyciela).
- Włącz **uwierzytelnianie dwuskładnikowe (2FA)** w **Settings → Password and authentication**. Chroni konto, nawet gdy ktoś pozna
  Twoje hasło. **Zapisz kody odzyskiwania** w bezpiecznym miejscu.
- **Nigdy nie udostępniaj** hasła, kodów 2FA ani tokenów.
- Twój **profil** (`github.com/twoj-login`) jest wizytówką: warto dodać zdjęcie, imię i krótki opis.

## Repozytorium na GitHubie

Strona repozytorium (np. `github.com/organizacja/repozytorium`) ma kilka zakładek:

| Zakładka | Do czego służy |
|---|---|
| **Code** | pliki projektu, historia, przycisk **Code** (adres do `clone`) |
| **Issues** | zgłoszenia: błędy, pomysły, zadania |
| **Pull requests** | propozycje zmian do przejrzenia i scalenia |
| **Actions** | automatyczne uruchamianie testów i innych zadań |
| **Settings** | ustawienia (widoczne tylko dla administratora repozytorium) |

### Tworzenie repozytorium

1. Kliknij **+** w prawym górnym rogu → **New repository**.
2. Wpisz **nazwę** (bez spacji i polskich znaków, np. `moj-projekt`).
3. Wybierz **widoczność**:

   | Widoczność | Kto widzi |
   |---|---|
   | **Public** (publiczne) | **każdy** w internecie |
   | **Private** (prywatne) | tylko Ty i osoby, którym nadasz dostęp |
   | **Internal** (wewnętrzne) | tylko członkowie organizacji (w planach dla firm i szkół) |

4. Zaznacz **Add a README file** (żeby repozytorium nie było puste). Opcjonalnie dodaj `.gitignore` i licencję.
5. Kliknij **Create repository**.

W Mazowieckim Technikum Innowacji repozytoria uczniów tworzy za Ciebie **Classroom 50** (patrz niżej).

### README.md: wizytówka projektu

Plik **`README.md`** w głównym folderze repozytorium GitHub **wyświetla pod listą plików** jako sformatowaną stronę
(Markdown). Dobre README mówi: czym jest projekt, jak go uruchomić, jak z niego korzystać. To pierwsza rzecz, którą widzi
każdy, kto wejdzie do repozytorium.

Markdown w pigułce:

| Zapis | Efekt |
|---|---|
| `# Nagłówek` | duży nagłówek (więcej `#` = mniejszy) |
| `**pogrubienie**`, `*kursywa*` | **pogrubienie**, *kursywa* |
| `- punkt` | lista punktowana |
| `` `kod` `` | kod w tekście |
| ` ``` ` na początku i końcu bloku | blok kodu |
| `[tekst](https://adres)` | link |
| `\| a \| b \|` z linią `\|---\|---\|` | tabela |

---

## Issues: zgłoszenia

**Issue** to zgłoszenie w repozytorium: błąd („program się wysypuje po wpisaniu liter"), pomysł („dodajmy tryb ciemny") albo
zadanie do zrobienia. Można je komentować, przypisywać osobom, oznaczać **etykietami** (*labels*) i zamykać po wykonaniu.

W opisie commitu lub pull requesta możesz napisać **`Closes #12`** (albo `Fixes #12`). Po scaleniu GitHub **sam zamknie** zgłoszenie
numer 12.

---

## Pull request: propozycja zmian

**Pull request** (PR) to **prośba o włączenie Twoich zmian** (z jednej gałęzi) do innej (zwykle `main`). To główny sposób
pracy zespołowej na GitHubie: zmiany są **przeglądane i komentowane**, zanim wejdą do projektu.

### Przebieg (GitHub flow)

```
1. git switch -c nowa-funkcja        ← tworzysz gałąź
2. (zmieniasz pliki) git add, git commit
3. git push -u origin nowa-funkcja   ← wysyłasz gałąź na GitHub
4. na GitHubie: Compare & pull request → wpisujesz tytuł i opis → Create pull request
5. koledzy przeglądają zmiany, komentują, proponują poprawki
6. poprawiasz, robisz kolejne commity i push (PR aktualizuje się sam)
7. po akceptacji: Merge pull request
8. usuwasz gałąź (przycisk Delete branch); u siebie: git switch main, git pull
```

**Co widać w pull requeście:**

| Zakładka | Zawartość |
|---|---|
| **Conversation** | opis, komentarze, status testów |
| **Commits** | lista commitów w PR |
| **Files changed** | **porównanie zmian linia po linii** (można komentować pojedyncze linie) |
| **Checks** | wyniki automatycznych testów (Actions) |

### Sposoby scalenia

Przy przycisku **Merge** możesz wybrać:

| Opcja | Co robi z historią |
|---|---|
| **Create a merge commit** | zachowuje wszystkie commity i dodaje commit scalający |
| **Squash and merge** | **zlewa wszystkie commity PR w jeden** (czysta historia) |
| **Rebase and merge** | przekłada commity na wierzch `main`, bez commitu scalającego |

Na początek wystarczy **Create a merge commit** albo **Squash and merge**.

### Przegląd kodu (*code review*)

Recenzent może: **zatwierdzić** (*Approve*), **zażądać zmian** (*Request changes*) albo tylko **skomentować**. Dobry przegląd to
rozmowa: pytaj, dlaczego coś jest zrobione tak, a nie inaczej, i proponuj konkretne poprawki.

---

## Fork: własna kopia cudzego projektu

**Fork** to **Twoja kopia cudzego repozytorium** na Twoim koncie GitHub. Używa się go, gdy chcesz zaproponować zmianę
w projekcie, do którego **nie masz prawa zapisu** (np. programy open source):

1. **Fork** → powstaje kopia na Twoim koncie,
2. **sklonuj** swojego forka, wprowadź zmiany, **wypchnij** je,
3. otwórz **pull request** z forka do oryginalnego repozytorium.

> W organizacji szkolnej **forkowanie prywatnych repozytoriów jest wyłączone**. Jeśli przy `git push` zobaczysz komunikat
> o braku uprawnień i propozycję „fork", prawdopodobnie próbujesz zapisać w repozytorium, do którego nie masz prawa
> (np. w szablonie zadania zamiast we **własnym** repozytorium). Sklonuj właściwe repozytorium.

---

## GitHub Actions: automatyczne zadania

**GitHub Actions** uruchamia **skrypty automatycznie po zdarzeniach** w repozytorium, np. po każdym `push`. Skrypty (zwane
**workflow**) zapisane są w plikach YAML w folderze `.github/workflows/`.

Do czego służą:

- **testy automatyczne:** po każdym pushu uruchamiają się testy, a Ty widzisz zielony znaczek albo czerwony krzyżyk,
- budowanie programu, publikowanie stron,
- w szkole: **automatyczne ocenianie zadań**.

Wynik zobaczysz w zakładce **Actions** (lista uruchomień) oraz **przy commicie** (zielona kropka/znaczek lub czerwony
krzyżyk). Po kliknięciu w uruchomienie zobaczysz **log**: co zostało wykonane i gdzie wystąpił błąd.

> Pliki w `.github/workflows` i `.classroom50.yaml` w repozytoriach z zadań **nie wolno zmieniać**: to one uruchamiają
> ocenianie.

---

## GitHub Classroom 50 w szkole

W naszej szkole nauczyciel wysyła **link do zadania**. Po jego kliknięciu i potwierdzeniu GitHub tworzy **Twoje własne
prywatne repozytorium** z plikami startowymi. Pracujesz w nim (`clone`, `commit`, `push`), a **po każdym pushu** testy
automatycznie sprawdzają zadanie i dają wynik. Szczegóły: [Classroom 50 dla uczniów](https://mazowieckie-technikum-innowacji.github.io/materialy-programowanie/classroom50-dla-uczniow.html).

---

## Inne przydatne rzeczy na GitHubie

| Funkcja | Do czego |
|---|---|
| **Releases** | oznaczone wersje projektu (na podstawie tagów) z plikami do pobrania |
| **GitHub Pages** | darmowa strona internetowa publikowana z repozytorium |
| **Projects** | tablice (kanban) do planowania zadań |
| **Discussions** | forum rozmów wokół projektu |
| **Gwiazdka (Star)** | zakładka „podoba mi się" dla repozytorium |
| **Wyszukiwarka** | `/` w GitHubie otwiera wyszukiwarkę kodu, repozytoriów i zgłoszeń |

## Pliki, które warto mieć w repozytorium

| Plik | Po co |
|---|---|
| `README.md` | opis projektu |
| `.gitignore` | lista plików, których Git ma nie śledzić ([rozdział 9](09-dobre-praktyki.md)) |
| `LICENSE` | licencja: co wolno robić z Twoim kodem |

---

⬅️ [7. Zdalne repozytorium](07-zdalne-repozytorium.md) · ➡️ [9. Dobre praktyki](09-dobre-praktyki.md)
