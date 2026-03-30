Ten dokument opisuje proces wytwórczy, którego się trzymamy, aby zapewnić wysoką jakość kodu i spójną historię zmian.

##  Workflow

Stosujemy model **Feature Branching**. Praca nad każdą zmianą odbywa się na dedykowanej gałęzi.

1. **Pobierz najnowsze zmiany:** Zawsze zaczynaj od `git pull origin main`.
2. **Stwórz nową gałąź:** Nazewnictwo gałęzi jest wymuszane (szczegóły poniżej).
3. **Wprowadź zmiany:** Pamiętaj o zasadach SOLID i strukturze Vertical Slice Architecture.
4. **Wyślij Pull Request:** Wypełnij szablon i poczekaj na recenzję Code Ownera.

## Standardy nazewnictwa branchy

Nazwa gałęzi powinna odzwierciedlać typ zmiany, przykładowo:
- `feat/krótki-opis` – nowa funkcjonalność.
- `fix/krótki-opis` – poprawa błędu.
- `docs/krótki-opis` – zmiany w dokumentacji.
- `refactor/krótki-opis` – poprawa struktury kodu bez zmiany logiki.

## Komunikaty commitów

Używamy standardu **Conventional Commits**. Pozwala to na automatyczne generowanie list zmian (changelogs).

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
4. **Czysty Kod:** Kod powinien być czytelny i sformatowany zgodnie z przyjętymi w projekcie regułami. Komentarze wewnątrz metod (inline) ograniczamy do minimum, skupiając się na jasnym nazewnictwie zmiennych i ustrukturyzowanej dokumentacji nagłówkowej.

## Proces Pull Request i Code Review

1. Po otwarciu PR, GitHub Actions automatycznie sprawdzi testy i formatowanie. Jeśli testy "nie przejdą", PR nie zostanie rozpatrzony.
2. Każdy projekt ma swojego **Lidera (Code Owner)**. Jego zatwierdzenie jest wymagane do połączenia kodu.
3. Stosujemy metodę **Squash and Merge**. Twoja cała historia commitów z gałęzi zostanie połączona w jeden czysty commit na gałęzi `main`.
