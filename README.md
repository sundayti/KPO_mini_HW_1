# Zoo Application

## Описание
Данное консольное приложение разработано для учёта животных и инвентарных объектов Московского зоопарка. Приложение позволяет:
- Добавлять новых животных после проверки их состояния здоровья ветеринарной клиникой.
- Формировать отчёты по количеству животных и их потреблению еды.
- Выводить список животных, подходящих для контактного зоопарка.
- Вести учёт инвентарных объектов (животных и вещей) с использованием уникальных идентификаторов (GUID).

## Архитектура и принципы
### Доменная модель и SOLID
- **Single Responsibility**: Каждый класс отвечает за свою задачу – например, `Animal` отвечает только за данные животного, `Zoo` — за учёт инвентарных объектов, а `VeterinaryClinic` — за проверку здоровья.
- **Open/Closed**: Базовые классы (например, `Animal`, `Thing`) расширяются через наследование (например, `Monkey`, `Rabbit`, `Tiger`, `Wolf`) без изменения исходного кода.
- **Liskov Substitution**: Все наследники базовых классов могут использоваться вместо своих базовых типов.
- **Interface Segregation**: Интерфейсы `IAlive` и `IInventory` разделяют ответственность за живые сущности и инвентаризацию.
- **Dependency Inversion**: Классы зависят от абстракций (например, `IVeterinaryClinic`, `IInventoryNumberGenerator`, `IInputService`, `IOutputService`), что упрощает замену реализаций и тестирование.

### DI-контейнер
Используется пакет `Microsoft.Extensions.DependencyInjection` для регистрации и внедрения зависимостей. Все ключевые сервисы (доменные и UI) регистрируются в DI-контейнере, что облегчает модульное тестирование и расширяемость системы.

### Паттерны Builder и Director
Для создания животных применяется паттерн **Builder**:
- **IAnimalBuilder** определяет методы `SetFood` и `SetGoodness` для установки параметров.
- Конкретные билдеры (`MonkeyBuilder`, `RabbitBuilder`, `TigerBuilder`, `WolfBuilder`) реализуют создание конкретных животных.
- Класс **AnimalDirector** управляет процессом сборки объекта, вызывая методы билдера в нужной последовательности (например, для травоядных животных через `ConstructHerboAnimal` и для хищников через `ConstructPredatorAnimal`).

### Тестирование
Приложение покрыто юнит‑тестами с использованием xUnit и Moq (покрытие тестами ZooDomain 83%):
- Тестируется логика доменной модели (валидаторы в конструкторах).
- Покрываются сценарии работы зоопарка (`Zoo`), UI-сервисов и класса `ZooApplication`.
- Отдельно покрываются тесты для билдера и директора, проверяя корректность построения объектов с заданными параметрами.

## Инструкция по запуску
1. `git clone https://github.com/sundayti/KPO_miniHW_1.git`
2. `cd Zoo`
3. `dotnet run`
