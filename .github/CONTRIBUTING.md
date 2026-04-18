Ten dokument opisuje proces wytwórczy, którego się trzymamy, aby zapewnić wysoką jakość kodu i spójną historię zmian.

##  Workflow

Stosujemy model Feature Branching z rygorystyczną kontrolą jakości.

1. **Pobierz najnowsze zmiany:** Zawsze zaczynaj od `git pull origin main`.
2. **Stwórz nową gałąź:** Nazewnictwo gałęzi jest wymuszane (szczegóły poniżej).
3. **Wprowadź zmiany:** Pamiętaj o zasadach SOLID i strukturze Vertical Slice Architecture.
4. **Auto-formatting**: Po wypchnięciu kodu (git push), bot automatycznie poprawi formatowanie i styl, jeśli zajdzie taka potrzeba.
5. **Wyślij Pull Request**: Wypełnij szablon, nadaj odpowiednią etykietę i poczekaj na recenzję Code Ownera.

## Standardy nazewnictwa branchy

System dopuszcza tylko gałęzie nazwane według wzorca: `<type>/<description-kebab-case>`.

- `feat` lub `feature` – nowa funkcjonalność
- `fix` – poprawa błędu
- `docs` – dokumentacja
- `refactor` – poprawa struktury kodu
- `test` – dodawanie/zmiana testów
- `chore`, `build`, `ci` – sprawy techniczne i konfiguracyjne

## Komunikaty commitów

Używamy **Conventional Commits**. Tytuł Twojego Pull Requesta musi zaczynać się od jednego z powyższych typów (np. `feat: add user profile slice`).

Struktura: `typ: opis`
- `feat: add user authentication module`
- `fix: resolve null pointer in payment process`
- `docs: update solid principles guide`

## Standardy techniczne

Przed wysłaniem kodu upewnij się, że spełnia on poniższe kryteria jakościowe:

1. **Vertical Slice Architecture:** Kod musi być odizolowany w obrębie odpowiedniego folderu funkcjonalności.
2. **SOLID:** Klasy i funkcje powinny posiadać jedną odpowiedzialność i być przygotowane pod testy jednostkowe/integracyjne.
3. **Dokumentacja Techniczna:**
   - Wszystkie publiczne metody, klasy oraz interfejsy muszą posiadać ustrukturyzowaną dokumentację techniczną (np. **XML Comments** dla C#, **Docstrings** dla Pythona).
   - Dokumentacja powinna opisywać przeznaczenie parametrów, zwracane wartości oraz możliwe wyjątki.
   - Celem jest umożliwienie automatycznego generowania specyfikacji (np. OpenAPI/Swagger) oraz wsparcie dla IntelliSense w środowiskach programistycznych.
4. **Automatyczny Styl**: Jeśli bot dokona poprawki stylu, zobaczysz commit od `github-actions[bot]`. Musisz pobrać te zmiany lokalnie (`git pull`), zanim będziesz mógł kontynuować pracę na tej gałęzi.

## Proces Pull Request i Code Review

1. **Status Checks**: PR musi zaliczyć 4 etapy: naming, format, build oraz test. Jeśli któryś zawiedzie, merge jest zablokowany.
2. **Versioning Labels**: Każdy PR musi posiadać dokładnie jedną etykietę: major, minor lub patch. Bez niej automat nie pozwoli na złączenie zmian.
3. **Stale Reviews**: Każdy nowy commit na gałęzi anuluje poprzednie zatwierdzenia lidera. Jeśli coś poprawisz – lider musi sprawdzić kod ponownie.
4. **Resolved Conversations**: Wszystkie dyskusje w Code Review muszą zostać oznaczone jako rozwiązane przed mergem.
5. **Linear History**: Stosujemy Squash and Merge. Twoja historia zostanie spłaszczona do jednego, czystego commita na main.