~~~mermaid
sequenceDiagram
    autonumber
    actor Admin as Администратор
    participant UI as Web UI Генератора
    participant Gen as Generator Service
    participant Broker as Message Broker

    Admin->>UI: Мгновенно списать 5 ГБ для User #123
    UI->>Gen: POST /api/v1/generator/events/oneshot {user_id: 123, amount: 5.0, resource_type: TRAFFIC}
    
    Gen->>Gen: Сформировать ConsumptionEvent
    Gen->>Broker: Publish(ConsumptionEvent)
    Gen-->>UI: 202 Accepted
    
    Note over Broker: Далее событие обрабатывается по стандартному пути (см. background_event_generation)
~~~