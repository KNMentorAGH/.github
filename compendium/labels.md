# System etykiet

Używamy etykiet do kategoryzacji zadań oraz automatyzacji procesów CI/CD.

## Typy zadań
- **`bug`** – Coś nie działa. Zgłoszenia błędów w kodzie lub infrastrukturze.
- **`feature`** – Nowa funkcjonalność lub znaczące ulepszenie istniejącej opcji.
- **`docs`** – Zmiany w dokumentacji (pliki `.md`, komentarze techniczne, instrukcje).
- **`dependencies`** – Automatyczne lub ręczne aktualizacje bibliotek i zewnętrznych paczek.

## 📈 Waga zmian (Versioning)
Używamy tych etykiet, aby określić wpływ zmiany na projekt:
- **`major`** – Zmiana krytyczna (Breaking Change). Kod po tej zmianie nie jest kompatybilny wstecz.
- **`minor`** – Nowa funkcjonalność dodana w sposób bezpieczny dla reszty systemu.
- **`patch`** – Drobne poprawki błędów lub drobne zmiany techniczne.