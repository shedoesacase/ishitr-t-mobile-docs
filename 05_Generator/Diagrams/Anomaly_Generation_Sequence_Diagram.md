~~~mermaid
sequenceDiagram
    autonumber
    actor Admin as Администратор
    participant UI as Web UI Генератора
    participant Gen as Generator Service
    participant Broker as Message Broker
    participant Core as Backend Core

    Admin->>UI: Запустить аномалию: User№123
    UI->>Gen: POST /api/v1/generator/events/anomaly/user/start {user_id = 123, resource_type = TRAFFIC, user_limits = [{resource_type: TRAFFIC, limit_value: 10}]}
    Gen->>Gen: Рассчитать объём (15–50% от limit_value) и скорость
    Gen-->>UI: 201 Created
    loop В течение 6 виртуальных часов (с учетом ускорения)
        Gen->>Gen: Сформировать чанк события
        Gen->>Broker: Publish(ConsumptionEvent) {is_anomaly: true, amount: chunk}
    end

    Broker->>Core: Consume(ConsumptionEvent)
    Core->>Core: Применить логику BR-15 (Детекция аномального роста)
    Core->>Core: Триггерить уведомление Владельцу (UC-08)
~~~