# Standardy nazewnictwa gałęzi

## Format nazwy
Każda nowa gałąź musi spełniać poniższy wzorzec:

`typ/krótki-opis`

### Zasady ogólne
- Używamy wyłącznie małych liter (**lowercase**).
- Zamiast spacji używamy myślnika (`-`).
- Opis powinien być zwięzły i techniczny.
- Unikamy nazw typu `poprawka`, `test1`, `z-zajec-final`.

## Prefixes

| Prefix | Przeznaczenie | Przykład |
| :--- | :--- | :--- |
| `feat/` | Nowa funkcjonalność (feature) | `feat/user-login-api` |
| `fix/` | Poprawa błędu (bugfix) | `fix/connection-timeout` |
| `docs/` | Zmiany tylko w dokumentacji. | `docs/update-vsa-guide` |
| `refactor/` | Zmiana struktury kodu, nie logiki | `refactor/cleanup-database-logic` |
| `chore/` | Zadania administracyjne | `chore/update-dependencies` |
| `test/` | Dodawanie lub poprawa testów | `test/add-integration-tests-vsa` |

---

## 💡 Dlaczego to jest ważne?

1. **Automatyzacja CI/CD:** Nasze potoki GitHub Actions mogą automatycznie uruchamiać inne zestawy testów dla `feat/` (np. pełne E2E), a inne dla `docs/` (tylko linter tekstu).
2. **Czytelność:** Szef projektu (Code Owner) widząc listę aktywnych gałęzi, od razu wie, nad czym pracuje zespół.


> **Pro-tip:** Jeśli masz problem z nazwaniem gałęzi, prawdopodobnie próbujesz zrobić zbyt wiele rzeczy naraz. Rozważ rozbicie zadania na mniejsze fragmenty zgodnie z zasadą **Vertical Slice Architecture**.