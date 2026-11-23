# Pytania kontrolne z odpowiedziami

Sprawdź się. Najpierw odpowiedz **sam**, potem rozwiń odpowiedź (kliknij w pytanie).

## Podstawy (rozdział 1-2)

<details>
<summary><b>1.</b> Czym różni się Git od GitHuba?</summary>

**Git** to program na Twoim komputerze, który zapisuje historię zmian w projekcie (działa bez internetu). **GitHub** to strona
internetowa (usługa), która przechowuje kopie repozytoriów Gita w chmurze i dodaje narzędzia do współpracy (pull requesty,
zgłoszenia, automatyczne testy).
</details>

<details>
<summary><b>2.</b> Wymień trzy obszary Gita i opisz, co jest w każdym.</summary>

**Katalog roboczy** (pliki, które edytujesz), **poczekalnia** (*staging area*: zmiany wybrane do najbliższego commitu),
**repozytorium** (zapisana historia commitów). Polecenie `git add` przenosi zmiany z katalogu roboczego do poczekalni, a
`git commit` z poczekalni do historii.
</details>

<details>
<summary><b>3.</b> Po co jest poczekalnia?</summary>

Pozwala **wybrać**, które zmiany trafią do commitu. Możesz zmienić pięć plików, a zapisać tylko dwa powiązane z jedną sprawą,
dzięki czemu **jeden commit opisuje jedną logiczną zmianę**, a historia jest czytelna.
</details>

<details>
<summary><b>4.</b> Do czego służy <code>git config --global user.name</code> i dlaczego trzeba je ustawić?</summary>

Ustawia imię i nazwisko, które Git zapisuje jako **autora** każdego commitu (podobnie `user.email`). Bez tego commity miałyby
niejasnego autora. Opcja `--global` oznacza, że ustawienie dotyczy wszystkich repozytoriów na tym komputerze.
</details>

## Commity i historia (rozdział 3-4)

<details>
<summary><b>5.</b> Co robi <code>git status</code> i kiedy go używać?</summary>

Pokazuje, na której jesteś gałęzi, które pliki są **nieśledzone**, **zmienione** albo **w poczekalni**. Używaj go **często**,
zwłaszcza przed `git add` i `git commit` oraz zawsze, gdy nie wiesz, co się dzieje.
</details>

<details>
<summary><b>6.</b> Czym różni się <code>git diff</code> od <code>git diff --staged</code>?</summary>

`git diff` pokazuje zmiany, **których jeszcze nie dodałeś** do poczekalni (katalog roboczy kontra poczekalnia).
`git diff --staged` pokazuje zmiany **już dodane** do poczekalni, czyli to, co wejdzie do najbliższego commitu.
</details>

<details>
<summary><b>7.</b> Co oznacza <code>HEAD~1</code>?</summary>

Commit **o jeden wcześniejszy** niż bieżący (`HEAD`). `HEAD~2` to o dwa wcześniejszy.
</details>

<details>
<summary><b>8.</b> Co robi <code>git add .</code> i jakie jest ryzyko?</summary>

Dodaje do poczekalni **wszystkie zmiany** w bieżącym folderze i podfolderach (nowe, zmienione, usunięte pliki). Ryzyko: możesz dodać
**niechciane pliki** (foldery `bin`/`obj`, hasła w `.env`). Dlatego przed `commit` sprawdzaj `git status`, a niechciane pliki wpisuj do `.gitignore`.
</details>

<details>
<summary><b>9.</b> Jak cofnąć zmiany w pliku, których jeszcze nie zapisałeś w commicie? Jakie jest ryzyko?</summary>

`git restore plik`. Ryzyko: **jest nieodwracalne**, zmiany znikają bez możliwości odzyskania (nie ma ich w koszu).
</details>

<details>
<summary><b>10.</b> Czym różni się <code>git reset --hard</code> od <code>git revert</code>? Kiedy którego używać?</summary>

`reset --hard` **przesuwa** gałąź na starszy commit i **kasuje** zmiany z cofniętych commitów (przepisuje historię), więc stosuj go tylko
na commitach **jeszcze niewysłanych**. `revert` tworzy **nowy commit odwracający** zmiany starego, nie kasuje historii, więc jest
**bezpieczny także po `push`**.
</details>

<details>
<summary><b>11.</b> Zrobiłeś <code>git reset --hard HEAD~2</code> przez pomyłkę. Jak odzyskać commity?</summary>

Przez `git reflog` znajdź wpis sprzed `reset` (np. `HEAD@{1}`) i wykonaj `git reset --hard HEAD@{1}`. `reflog` zapisuje, gdzie wskazywał
`HEAD`, nawet jeśli commity zniknęły z historii.
</details>

<details>
<summary><b>12.</b> Kiedy możesz użyć <code>git commit --amend</code>, a kiedy nie?</summary>

Możesz poprawić **ostatni** commit (opis albo dodać zapomniany plik), **o ile jeszcze nie wysłałeś go** (`push`). Po `push` nie, bo
`amend` tworzy nowy commit w miejsce starego (zmienia identyfikator) i przepisałby historię, którą inni mogą mieć.
</details>

## Gałęzie, scalanie, konflikty (rozdział 5-6)

<details>
<summary><b>13.</b> Czym jest gałąź w Gicie?</summary>

Jest to **wskaźnik (etykieta) na jeden commit**. Dzięki temu tworzenie gałęzi jest błyskawiczne. `HEAD` wskazuje gałąź, na której pracujesz,
a nowy commit przesuwa tę gałąź do przodu.
</details>

<details>
<summary><b>14.</b> Czym różni się <code>git switch nazwa</code> od <code>git switch -c nazwa</code>?</summary>

`git switch nazwa` przełącza na **istniejącą** gałąź. `git switch -c nazwa` **tworzy** nową gałąź i od razu się na nią przełącza.
</details>

<details>
<summary><b>15.</b> Na jakiej gałęzi musisz być, wykonując <code>git merge nowa-funkcja</code>, i co się stanie?</summary>

Musisz być na gałęzi, **do której chcesz wciągnąć zmiany** (zwykle `main`). `git merge X` wciąga gałąź `X` **do bieżącej**.
</details>

<details>
<summary><b>16.</b> Czym różni się scalenie <i>fast-forward</i> od commitu scalającego?</summary>

**Fast-forward**: gałąź docelowa nie miała nowych commitów, więc Git po prostu przesuwa jej wskaźnik do przodu (bez nowego commitu).
**Commit scalający**: obie gałęzie mają własne commity, więc Git tworzy **nowy commit z dwoma rodzicami**.
</details>

<details>
<summary><b>17.</b> Co oznaczają znaczniki <code>&lt;&lt;&lt;&lt;&lt;&lt;&lt; HEAD</code>, <code>=======</code> i <code>&gt;&gt;&gt;&gt;&gt;&gt;&gt; b1</code> w pliku?</summary>

To **konflikt scalania**. Między `<<<<<<< HEAD` a `=======` jest wersja z **bieżącej** gałęzi, a między `=======` a `>>>>>>> b1` wersja z
**wciąganej** gałęzi `b1`. Musisz zdecydować, co zostaje, **usunąć znaczniki**, zrobić `git add plik` i `git commit`.
</details>

<details>
<summary><b>18.</b> Pomyliłeś się przy rozwiązywaniu konfliktu i chcesz zacząć od nowa. Co robisz?</summary>

`git merge --abort`: przerywa scalanie i wraca do stanu sprzed `merge`. Konflikt wróci przy następnej próbie scalenia.
</details>

## Zdalne repozytorium i GitHub (rozdział 7-8)

<details>
<summary><b>19.</b> Co robi <code>git clone</code> i ile razy go używasz dla jednego projektu?</summary>

Pobiera **całe repozytorium** (z historią) z GitHuba do nowego folderu i zapisuje jego adres jako `origin`. Dla jednego projektu na jednym komputerze robisz to **raz**.
</details>

<details>
<summary><b>20.</b> Co to jest <code>origin</code>?</summary>

Umowna **nazwa zdalnego repozytorium** (skrót zamiast długiego adresu). Po `git clone` wskazuje repozytorium, z którego sklonowałeś projekt.
</details>

<details>
<summary><b>21.</b> Co robi <code>git push -u origin main</code> i po co jest <code>-u</code>?</summary>

Wysyła commity z lokalnej gałęzi `main` do zdalnego `origin` i **zapamiętuje powiązanie** (`-u` = `--set-upstream`), dzięki czemu
kolejne `git push` i `git pull` działają bez podawania argumentów.
</details>

<details>
<summary><b>22.</b> Czym różni się <code>git fetch</code> od <code>git pull</code>?</summary>

`git fetch` **pobiera** nowe commity z GitHuba, ale **nie zmienia** Twoich plików ani gałęzi. `git pull` to `fetch` **plus włączenie** (merge) pobranych zmian do bieżącej gałęzi.
</details>

<details>
<summary><b>23.</b> <code>git push</code> został odrzucony („rejected, fetch first"). Co to znaczy i co robisz?</summary>

Na GitHubie są commity, **których nie masz u siebie**. Git nie pozwala nadpisać cudzej pracy. Robisz `git pull` (ewentualnie rozwiązujesz konflikt), a potem `git push`.
**Nie** używasz `--force`, bo to skasowałoby cudzą pracę.
</details>

<details>
<summary><b>24.</b> Co to jest pull request i jakie są jego główne kroki?</summary>

To **prośba o włączenie zmian z jednej gałęzi do innej** (zwykle do `main`), z możliwością przeglądu i komentarzy. Kroki: nowa gałąź → commity → `push` gałęzi → otwarcie
PR na GitHubie → przegląd i poprawki → **Merge** → usunięcie gałęzi.
</details>

<details>
<summary><b>25.</b> Do czego służy <code>.gitignore</code>? Dlaczego dopisanie pliku, który już jest w repozytorium, nie wystarcza?</summary>

To lista wzorców plików, których Git ma **nie śledzić** (np. `bin/`, `.env`). Działa tylko na pliki **jeszcze nieśledzone**. Jeśli plik już jest w repozytorium,
trzeba go odpiąć: `git rm --cached plik` i zrobić commit.
</details>

<details>
<summary><b>26.</b> Przez pomyłkę wrzuciłeś do publicznego repozytorium plik z hasłem. Co robisz najpierw?</summary>

**Natychmiast zmieniasz (unieważniasz) hasło**. Samo usunięcie pliku nie wystarczy, bo sekret zostaje w historii commitów, a boty skanują GitHuba w poszukiwaniu takich danych.
</details>

<details>
<summary><b>27.</b> W zadaniu z Classroom 50 co jest „oddaniem pracy" i gdzie zobaczysz wynik?</summary>

Każdy `push` na `main` w Twoim repozytorium z zadaniem (wyjątek: zadania, w których opis mówi o tagu). Wynik zobaczysz w zakładce **Actions**, przy commicie (znaczek) i w pull requeście **Feedback**.
</details>

---

⬅️ [Ćwiczenia](cwiczenia.md) · [Spis](README.md)
