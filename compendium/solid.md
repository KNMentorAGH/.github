# Ściąga: Jak pisać wysokiej jakości, testowalny, łatwy w utzymaniu, testowalny i czytelny kod

## 1. Single Responsibility Principle (SRP)

*Klasa powinna mieć tylko jeden powód do zmiany.*

### ❌ Żle

`OrderProcessor` zarządza walidacją, zapisem do bazy, autoryzacją płatności oraz wysyłką powiadomień. Zmiana formatu e-maila wymusza modyfikację tej samej klasy, co logika procesowania zamówień.

```python
class OrderProcessor:
    def process(self, order):
        if order.is_valid():
            db_connection.save(order)
            payment_gateway.charge(order.total)
            email_service.send_confirmation(order.customer_email)
```

### ✅ Lepiej

Rozdzielenie odpowiedzialności na dedykowane serwisy. `OrderProcessor` staje się orkiestratorem.

```python
class OrderRepository:
    def save(self, order):
        pass

class PaymentService:
    def charge(self, amount):
        pass

class NotificationService:
    def notify(self, email):
        pass

class OrderProcessor:
    def __init__(self, repository, payment, notification):
        self.repository = repository
        self.payment = payment
        self.notification = notification

    def process(self, order):
        self.repository.save(order)
        self.payment.charge(order.total)
        self.notification.notify(order.customer_email)
```

## 2. Open/Closed Principle (OCP)

*Klasy powinny być otwarte na rozszerzenia, ale zamknięte na modyfikacje.*

### ❌ Żle

Dodanie nowego typu powiadomienia (np. powiadomienia push) wymaga edycji metody send i dodania kolejnego bloku if/elif.

```python
class NotificationManager:
    def send(self, message, channel):
        if channel == "email":
            self.send_email(message)
        elif channel == "sms":
            self.send_sms(message)
```

### ✅ Lepiej

Zastosowanie polimorfizmu. Nowy kanał komunikacji wymaga stworzenia nowej klasy bez dotykania istniejącej logiki.

```python
from abc import ABC, abstractmethod

class MessageChannel(ABC):
    @abstractmethod
    def send(self, message):
        pass

class EmailChannel(MessageChannel):
    def send(self, message):
        pass

class SlackChannel(MessageChannel):
    def send(self, message):
        pass

class NotificationManager:
    def notify(self, message, channel: MessageChannel):
        channel.send(message)
```
## 3. Liskov Substitution Principle (LSP)

*Klasy pochodne muszą być w stanie zastąpić swoje klasy bazowe bez zmiany poprawności programu.*

### ❌ Żle

`ReadOnlySqlRepository` dziedziczy po `SqlRepository`, ale rzuca wyjątek przy próbie zapisu. Programista używający bazy nie spodziewa się awarii przy wywołaniu metody `save`.

```python
class SqlRepository:
    def save(self, data):
        db.insert(data)

class ReadOnlySqlRepository(SqlRepository):
    def save(self, data):
        raise Exception("Cannot write to read-only repository")
```

### ✅ Lepiej

Wydzielenie interfejsów tak, aby klasy implementowały tylko to, co faktycznie potrafią obsłużyć.

```python
class Readable(ABC):
    @abstractmethod
    def get_all(self):
        pass

class Writable(ABC):
    @abstractmethod
    def save(self, data):
        pass

class FullRepository(Readable, Writable):
    def get_all(self):
        pass
    def save(self, data):
        pass

class ReadOnlyRepository(Readable):
    def get_all(self):
        pass
```

## 4. Interface Segregation Principle (ISP)

Klient nie powinien być zmuszany do polegania na metodach, których nie używa.

### ❌ Żle

Interfejs `SmartDevice` wymusza na zwykłej żarówce implementację sterowania temperaturą.
```python
class SmartDevice(ABC):
    @abstractmethod
    def toggle_power(self):
        pass
    @abstractmethod
    def set_temperature(self):
        pass

class SmartBulb(SmartDevice):
    def toggle_power(self):
        pass
    def set_temperature(self):
        raise NotImplementedError()
```

### ✅ Lepiej

Podział na mniejsze, wyspecjalizowane interfejsy.

```python
class Switchable(ABC):
    @abstractmethod
    def toggle_power(self):
        pass

class Thermostat(ABC):
    @abstractmethod
    def set_temperature(self):
        pass

class SimpleBulb(Switchable):
    def toggle_power(self):
        pass

class SmartHeater(Switchable, Thermostat):
    def toggle_power(self):
        pass
    def set_temperature(self):
        pass
```

## 5. Dependency Inversion Principle (DIP)

Wysokopoziomowe moduły nie powinny zależeć od modułów niskopoziomowych. Oba powinny zależeć od abstrakcji.

### ❌ Żle

`DataArchiver` jest sztywno powiązany z konkretną klasą `LocalFileSystem`. Nie można go łatwo przetestować ani zmienić magazynu na AWS S3.

```python
class LocalFileSystem:
    def write(self, file):
        pass

class DataArchiver:
    def __init__(self):
        self.storage = LocalFileSystem()

    def archive(self, data):
        self.storage.write(data)
```

### ✅ Lepiej

`DataArchiver` zależy od abstrakcji `Storage`. Konkretna implementacja jest wstrzykiwana (Dependency Injection).

```python
class Storage(ABC):
    @abstractmethod
    def write(self, data):
        pass

class S3Storage(Storage):
    def write(self, data):
        pass

class DataArchiver:
    def __init__(self, storage: Storage):
        self.storage = storage

    def archive(self, data):
        self.storage.write(data)
```