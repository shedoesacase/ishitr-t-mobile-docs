~~~mermaid
sequenceDiagram
    autonumber
    actor Admin as Администратор
    participant UI as Web UI Генератора
    participant Gen as Generator Service
    participant Broker as Message Broker
    participant Core as Backend Core

    Admin->>UI: Вводит множитель 1000, жмет "Ускорить"
    UI->>Gen: POST /api/v1/generator/time/accelerate {multiplier: 1000}
    Gen->>Gen: Фиксирует base_real, base_virtual, multiplier
    Gen-->>UI: 200 OK (TimeStatus)
    UI->>UI: Блокирует поле, ускоряет анимацию часов

    rect rgb(240, 255, 240)
    Note over Gen,Core: Асинхронная синхронизация
    Gen->>Broker: Publish(TimeChangeEvent) {base_real, base_virtual, multiplier, is_paused}
    Broker->>Core: Consume(TimeChangeEvent)
    Core->>Core: Обновляет локальные параметры виртуальных часов
    end
~~~