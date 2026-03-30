# Jak robić Code Review?

Code Review to proces weryfikacji jakości technicznej kodu przed scaleniem z główną gałęzią. Traktujemy go jako barierę ochronną dla stabilności systemu oraz narzędzie wymiany wiedzy wewnątrz koła.

## 1. Perspektywa Autora

Zanim poprosisz Lidera (Code Ownera) o sprawdzenie kodu, wykonaj **Self-Review**:

1. **Szanuj czas innych:** Przed wysłaniem PR sam go przeczytaj. Czy wszystko jest w dobrych folderach? Czy nie brakuje komentarzy? Czy testy przechodzą?
2. **Opisz kontekst:** Skorzystaj z `PULL_REQUEST_TEMPLATE.md` i wskaż recenzentowi, gdzie jest sedno Twojej zmiany.

### 2. Perspektywa Recenzenta

Jako recenzent nie szukaj tylko literówek – od tego mamy Lintery. Skup się na architekturze i logice:

### A. Poprawność Architektoniczna (VSA & SOLID)
* Czy logika biznesowa nie wycieka do kontrolerów API?
* Czy klasy są małe i odpowiedzialne za jedną rzecz (SRP)?
* Czy zależności są wstrzykiwane przez konstruktor (DIP)?

### B. Czytelność i Naming
* Czy nazwy funkcji to czasowniki (np. `calculate_total`, `save_user`)?
* Czy nazwa zmiennej mówi, co ona przechowuje, a nie jakiego jest typu (unikaj `user_list`, stosuj `users`)?

### C. Testy i Brzegowe Przypadki
* Czy dodano test integracyjny dla "Happy Path"?
* Czy kod obsłuży sytuację, gdy baza zwróci `null` lub API zewnętrznego dostawcy nie odpowie (Timeouts)?

## 3. Decyzje i Blokowanie Merge

Recenzent ma trzy opcje zakończenia przeglądu:

1.  **Approve:** Kod spełnia standardy. Gotowy do Squash Merge.
2.  **Comment:** Sugestie kosmetyczne, które nie blokują wdrożenia. Autor może, ale nie musi ich wprowadzać.
3.  **Request Changes (BLOCK):** Bezwzględne wstrzymanie integracji.

**Kiedy blokujemy?**
* Błędy logiczne (np. zły algorytm naliczania zniżek).
* Brak wymaganych testów dla nowej funkcjonalności.
* Drastyczne łamanie struktury folderów (np. logika feature A w folderze feature B).
* Luki bezpieczeństwa (np. SQL Injection, brak autoryzacji w endpoincie).
