~~~mermaid
sequenceDiagram
    autonumber
    actor Admin as Администратор
    participant UI as Web UI Генератора
    participant Core as Backend Core API
    participant DB as Core Database

    Admin->>UI: Открывает вкладку "Дашборд"
    
    par Параллельная загрузка справочников (UI -> Core напрямую)
        UI->>Core: GET /api/v1/users
        Core->>DB: SELECT * FROM User
        DB-->>Core: rows
        Core-->>UI: 200 [User]
    and
        UI->>Core: GET /api/v1/groups
        Core->>DB: SELECT * FROM FamilyGroup
        DB-->>Core: rows
        Core-->>UI: 200 [FamilyGroup]
    and
        UI->>Core: GET /api/v1/tariffs
        Core->>DB: SELECT * FROM ServicePackage
        DB-->>Core: rows
        Core-->>UI: 200 [ServicePackage]
    end

    UI->>UI: Отрисовка таблиц и выпадающих списков
    Note over UI: Если данные пусты, показывается заглушка
~~~