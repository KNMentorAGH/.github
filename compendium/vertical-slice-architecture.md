# Ściąga: Jak dzielić kod między folderami?

Vertical Slice Architecture to podejście do budowy systemów, które rezygnuje z tradycyjnego podziału na warstwy techniczne (np. warstwa dostępu do danych, warstwa biznesowa) na rzecz podziału opartego na funkcjonalnościach systemu (tzw. "plastrach"). Każda funkcjonalność (slice) jest odizolowana i zawiera wszystkie elementy niezbędne do jej realizacji – od obsługi żądania po zapis w bazie danych.

## 1. Grupowanie według Funkcjonalności (Features)

*W tradycyjnej architekturze kod jest rozproszony po wielu folderach technicznych. W VSA wszystkie pliki dotyczące jednej funkcji znajdują się w jednym miejscu.*

### ❌ Żle

Podział poziomy (Horizontal Layers). Dodanie jednej funkcji wymaga modyfikacji plików w wielu odległych folderach.

```
📁 src/
├── 📁controllers/
│   └── user_controller.py
├── 📁services/
│   └── user_service.py
├── 📁models/
│   └── user_model.py
└── 📁repositories/
    └── user_repository.py
```

### ✅ Lepiej

Podział pionowy (Vertical Slices). Cała logika biznesowa, walidacja i dostęp do danych dla konkretnego zadania są zgrupowane.

```
📁src/
└── 📁features/
    ├── 📁register_new_user/
    │   ├── api_endpoint.py
    │   ├── logic.py
    │   └── data_access.py
    └── 📁get_user_profile/
        ├── api_endpoint.py
        └── profile_query.py
```

## 2. Unikanie nadmiernych abstrakcji

VSA promuje zasadę, że każda funkcja może mieć unikalne wymagania. Nie ma potrzeby tworzenia skomplikowanych interfejsów i generycznych serwisów, jeśli są one używane tylko przez jeden element systemu.

### ❌ Żle

Używanie generycznego repozytorium, które narzuca sztywną strukturę dla każdej funkcji, nawet jeśli wymagane jest tylko proste zapytanie SQL.

```python
# Współdzielone, skomplikowane repozytorium
class GenericRepository:
    def get_with_complex_joins(self, filters):
        # 200 linii generycznego kodu
        pass

# Funkcja pobierająca profil musi dostosować się do repozytorium
def get_user_profile(user_id):
    repo = GenericRepository()
    return repo.get_with_complex_joins({"id": user_id, "active": True})
```

### ✅ Lepiej

Implementacja logiki dostępu do danych bezpośrednio wewnątrz "plastra" funkcjonalności, dopasowana dokładnie do potrzeb danego zadania.

```python
# src/features/get_user_profile/profile_query.py

def get_user_profile_from_db(user_id: int):
    # Proste, dedykowane zapytanie SQL/ORM tylko dla tej funkcji
    return db.session.query(User).filter(User.id == user_id).first()
```
# 3. Minimalizacja współdzielenia kodu (Dry vs Coupling)

W VSA dopuszcza się powielenie drobnych elementów kodu między funkcjami, jeśli zapobiega to tworzeniu silnych powiązań (coupling) między nimi. Współdzielenie modeli danych między różnymi funkcjonalnościami (slices) może prowadizć do sytacji, że zmiana w jednym module nieoczekiwanie zepsuje inny.

### ❌ Żle

Używanie jednego, globalnego obiektu DTO (Data Transfer Object) dla wielu różnych operacji. Zmiana wymagań dla rejestracji (np. dodanie pola `marketing_consent`) wpływa na profil użytkownika, mimo że to pole nie jest tam potrzebne.

```python
# shared/models.py
class UserDTO(BaseModel):
    id: Optional[int]
    username: str
    password: str
    email: str
    # Zmiana tutaj wpływa na WSZYSTKIE funkcje używające UserDTO
```

### ✅ Lepiej

Każda funkcjonalność definiuje własne struktury danych, nawet jeśli początkowo wyglądają identycznie. Pozwala to na niezależną ewolucję każdego "plastra".

```python
# features/register_user/models.py
class RegisterUserRequest(BaseModel):
    username: str
    password: str
    email: str
    marketing_consent: bool # Pole specyficzne tylko dla rejestracji

# features/get_user_profile/models.py
class UserProfileResponse(BaseModel):
    username: str
    email: str
    # Brak pola password i marketing_consent - izolacja danych
```

## 4. Testowanie funkcjonalne

Testowanie w VSA skupia się na weryfikacji całego "plastra" (od wejścia do wyjścia), co lepiej odzwierciedla realne zachowanie systemu niż testowanie odizolowanych warstw technicznych.

### ✅ Zalecane podejście: Testy integracyjne funkcjonalności

Zaleca się pisanie testów, które operują na publicznym interfejsie danej funkcji (np. endpoint API) i korzystają z realnej infrastruktury (np. baza danych SQLite w pamięci lub kontener testowy). Dzięki temu testujemy realne zapytania SQL, walidację oraz logikę biznesową jednocześnie.

```python
# features/register_user/test_integration.py

def test_register_user_feature_full_flow(client):
    # 1. Przygotowanie danych wejściowych
    payload = {
        "username": "student_agh",
        "password": "secure_password123",
        "email": "student@agh.edu.pl"
    }

    # 2. Wywołanie pełnego przepływu funkcjonalności przez API
    response = client.post("/register", json=payload)
    
    # 3. Weryfikacja odpowiedzi serwera
    assert response.status_code == 201
    assert response.json()["username"] == "student_agh"

    # 4. Bezpośrednia weryfikacja stanu bazy danych (potwierdzenie efektu ubocznego)
    user_in_db = db.session.query(User).filter_by(username="student_agh").first()
    assert user_in_db is not None
    assert user_in_db.email == "student@agh.edu.pl"
```