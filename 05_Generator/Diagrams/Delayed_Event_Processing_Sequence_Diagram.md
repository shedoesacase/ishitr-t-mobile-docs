~~~mermaid
sequenceDiagram
    autonumber
    actor Admin as Администратор
    participant UI as Web UI Генератора
    participant Gen as Generator Service
    participant Broker as Message Broker
    participant Core as Backend Core
    participant DB as Core Database

    Admin->>UI: Отправить запоздалое событие (1.5 ГБ, 3 часа назад)
    UI->>Gen: POST /api/v1/generator/events/oneshot {user_id: 123, amount: 1.5, lateness_hours: 3}
    
    Gen->>Gen: Сформировать ConsumptionEvent
    Gen->>Broker: Publish(ConsumptionEvent)
    Gen-->>UI: 202 Accepted

    Broker->>Core: Consume(ConsumptionEvent)
    Core->>DB: Проверка идемпотентности
    
    Note over Core: Событие новое. Применяем BR-16
    Core->>Core: Игнорируем исходный event_timestamp
    Core->>Core: Привязываем событие к текущему виртуальному моменту
    Core->>DB: Списать 1.5 ГБ по текущему периоду
~~~