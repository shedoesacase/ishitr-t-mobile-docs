~~~mermaid
sequenceDiagram
    autonumber
    actor Admin as Администратор
    participant UI as Web UI Генератора
    participant Gen as Generator Service
    participant Broker as Message Broker
    participant Core as Backend Core
    participant DB as Core Database

    Admin->>UI: Запускает фон. генерацию User№123
    UI->>Gen: POST /api/v1/generator/events/background/user/start
    Gen-->>UI: 201 Created
    Note over Gen: Запуск внутреннего таймера

    loop Каждые 60 виртуальных секунд (задается в запросе)
        Gen->>Gen: Формирует ConsumptionEvent (UUID, amount, virtual_time)
        Gen->>Broker: Publish(ConsumptionEvent) [Fire & Forget]
    end

    Broker->>Core: Consume(ConsumptionEvent)
    Core->>DB: Проверка идемпотентности по event_id
    alt Событие уже обработано
        Core->>Core: Игнорировать (Дубликат)
    else Новое событие
        Core->>DB: Проверить остаток GroupPackageBalance / UserLimit
        alt Баланс достаточен
            Core->>DB: Списать трафик/минуты, обновить счетчики
            Core->>DB: Сохранить event_id в историю
        else Баланс недостаточен (Генератор об этом не знает)
            Core->>Core: Отклонить событие, логировать "Insufficient Balance"
            Note over Core: Списание не происходит, но Генератор продолжает слать
        end
    end
~~~

