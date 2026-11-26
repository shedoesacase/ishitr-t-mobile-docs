# Журнал изменений

Все значимые изменения в документации и артефактах проекта «Т-мобайл: Семейный тариф» документируются в этом файле.

---
## Неопубликованное
### ➕ Добавлено
* **Generator_Service_API.yaml:**
	* Добавлена аутентификация по API-ключу для всех эндпоинтов управления генератором.
	* Добавлены публичные эндпоинты `GET /health` и `GET /ready`.
	* В запрос запуска аномалии добавлено обязательное поле `user_limits` — массив снимков личных лимитов пользователя для расчёта объёма аномалии.
	* Добавлена схема `UserLimitSnapshot` в `components/schemas`.
* **Generator_Concept_and_Architecture.md:**
	* Уточнён сценарий аномального роста: генератор получает снимок личных лимитов и рассчитывает объём аномалии от 15 до 50% переданного лимита.
* **Mobile_App_API.yaml:**
	* Добавлен `GET /api/v1/users/search?phone=...` — поиск пользователя по номеру телефона для приглашения в группу.
	* Добавлен агрегированный `GET /api/v1/groups/{group_id}/limits` с ролевой фильтрацией.
	* Добавлен `PUT /api/v1/groups/{group_id}/limits` — обновление лимитов нескольких участников за один запрос (только OWNER).
	* В `ForecastResponse` добавлено обязательное поле `members` с персональными прогнозами участников: Владелец видит прогнозы всех участников, Участник — только свой.
	* Добавлены схемы: `UserSearchResult`, `UserSearchResponse`, `MemberLimits`, `GroupLimitsResponse`, `LimitUpdateItem`, `BatchUpdateLimitsRequest`, `MemberLimitResult`, `BatchUpdateLimitsResponse`, `MemberForecast`.
* **Backend_Core_API.yaml:**
	* Добавлен внутренний контракт для всех модулей: `GET /api/v1/users/search`, `GET/PUT /api/v1/groups/{group_id}/limits`, `GET /api/v1/groups/{group_id}/statistics`, `GET /api/v1/groups/{group_id}/forecast`, `GET /api/v1/groups/{group_id}/history`.
	* Добавлены схемы: `UserSearchResult`, `UserSearchResponse`, `UserLimitDetail`, `MemberLimits`, `GroupLimitsResponse`, `SetLimitRequest`, `LimitUpdateItem`, `BatchUpdateLimitsRequest`, `MemberLimitResult`, `BatchUpdateLimitsResponse`, `DailyUsageEntry`, `MemberUsageStat`, `StatisticsResponse`, `MemberForecast`, `ForecastResponse`, `AuditLogEntry`, `PaginationInfo`, `HistoryResponse`.
* **Mobile_App_API.yaml, Backend_Core_API.yaml:**
	* Добавлен `GET /api/v1/groups/{group_id}/periods` — список расчетных периодов группы с статусами `ACTIVE`/`ARCHIVED.
	* Добавлены схемы `PeriodInfo` и `PeriodListResponse`.

### 🔄 Изменено
- Единицы измерения трафика во всех документах приведены к единому типу (ГБ).
- **Use_Cases.md:**
	- В UC-01/UC-02/UC-03 теперь явно указано состояние персональных ограничений до установки лимита.
	- UC-13 переписан: смена периода выполняется автоматически по виртуальному времени из `TimeChangeEvent.
- **Backend_Core_API.yaml:**
	- `DELETE /api/v1/groups/{group_id}/members/{user_id}` заменён на `POST /api/v1/groups/{group_id}/members/{user_id}/exclude`: soft delete со статусом `EXCLUDED`, сохранением истории и записью в AuditLog.
-  **Mobile_App_API.yaml, Backend_Core_API.yaml, Generator_Service_API.yaml:**
	- В апи-контрактах также теперь явно указано состояние персональных ограничений до установки лимита.
- **Data_Dictionary.md:**
	- Статус лимитов до установки добавлен в словарь данных.
* Все REST-контракты теперь используют плоский `ErrorResponse` с полями `error_code`, `message`, `timestamp`, `trace_id` (обязательные) и опциональным `details`. 
---

## [0.1.0] — 25-09-2026

### ➕ Добавлено
* **Business_Requirements.md:**
	* Создан файл Business_Requirements.md.
	- Описано введение в проект, стейкхолдеры и границы проекта.
	- Описаны бизнес-правила.
* **Use_Cases.md:**
	* Создан файл Use_Cases.md.
	- Описаны Use Cases, покрывающие бизнес-правила (Файл Use_Cases.md).
- **Data_Dictionary.md**
	- Создан словарь данных, описывающий таблицы базы данных.

---

## [0.2.0] — 29-09-2026

### ➕ Добавлено
* **Business_Requirements.md:**
	- Добавлен BR-23.
	- Добавлено явное указание того, что управление финансами находится за границами проекта.
* **Data_Dictionary.md:**
	- Добавлено краткое описание каждой таблицы в словаре данных.
	- Добавлено описание связей таблиц в словаре данных.
- **02_Database/diagrams/...**
	- Добавлены диаграммы сущности отношений для всех доменов.
- **README.md**
- **CHANGELOG.md**
### 🔄 Изменено
* **Business_Requirements.md:**
	- Дополнен BR-07. Теперь более явно указано, как расчетный период нумеруется и используется в системе.
* **Data_Dictionary.md:**
	- В таблице 4 столбец owner_id теперь UNIQUE.
	- Из таблицы 8 убран столбец group_id.
	- Изменена связь family_group:group_member в домене 2.
	- Добавлены недостающие связи в домены 4 и 5.
	- В таблице 17 поля actor_user_id и target_user_id теперь FK на 1 таблицу.

---

## [0.2.1] — 16-10-2026
### ➕ Добавлено
* **Generator_Concept_and_Architecture.md:**
	* Создан файл Generator_Concept_and_Architecture.md
	* Описана архитектура и концепция генератора
* **Generator-Backend_API.yaml:**
	* Создан файл Generator-Backend_API.yaml
	* Описаны некоторые эндпоинты апи генератора
* **README.md:**
	* Добавлена ссылка на дизайн-концепт в фигме.
### 🔄 Изменено
* **Data_Dictionary.md:**
	- В таблице 8 поле is_active переименовано в поле is_blocked и теперь связано с UC-02.
* **Use_Cases.md:**
	* Отредактирован альтернативный сценарий 5а в UC-02.
* **Business_Requirements.md:**
	* Дополнен BR-18.

---

## [0.2.2] — 20-10-2026
### ➕ Добавлено
* **Backend_Core_API.yaml:**
	* Создан файл Backend_Core_API.yaml.
	* Описаны эндпоинты апи бэкэнда.
* **Generator_Service_API.yaml:**
	* Создан файл Generator_Service_API.yaml.
	* Описаны эндпоинты апи генератора.
* **Broker_Events_Contract.yaml:**
	* Создан файл Broker_Events_Contract.yaml.
	* Описаны схемы сообщений для брокера.
### 🔄 Изменено
* **Generator-Backend_API.yaml:**
	* Файл разбит на 3 отдельных файла и больше не существует.

## [0.2.3] — 01-11-2026
### ➕ Добавлено
* **Background_Generation_Event_Processing_Sequence_Diagram.md:**
	* Описана диаграмма последовательности обработки события потребления и фоновой генерации
* **Anomaly_Generation_Sequence_Diagram.md:**
	* Описана диаграмма последовательности обработки события аномального потребления.
* **Dashboard_Data_Loading_Sequence_Diagrammd:**
	* Описана диаграмма последовательности загрузки дашборда в админ-панели.
* **Delayed_Event_Processing_Sequence_Diagram.md:**
	* Описана диаграмма последовательности обработки запоздалого события.
* **Instant_Event_Generation_Sequence_Diagram.md:**
	* Описана диаграмма последовательности обработки мгновенного события.
* **Virtual_Clock_Synchronization_Sequence_Diagram.md:**
	* Описана диаграмма последовательности синхронизации времени между микросервисами.
* **Generator_UI_Scenarios.md:**
	* Подробно описаны сценарии использования генератора для создания web-страницы админ-панели.
* Добавлены диаграммы в нотации C4 для уровней Context и Containers
### 🔄 Изменено
* **Generator_Concept_and_Architecture.md:**
	- Описаны подробно сценарии
* **Use_Cases.md:**
	* Изменен UC-16: Добавлен альтернативный сценарий 4c.
* **Data_Dictionary.md:**
	* Добавлено поле name в таблицу user.
* **Generator_Concept_and_Architecture.md:**
	* Добавлено описание периодической генерации данных.
	* Добавлено уточнение о том, что время в системе связано по средствам брокера сообщений.
	* Добавлено описание массовой генерации.
* **Generator_Service_API.yaml:**
	* Обновлены методы генерации данных.
	* Добавлен метод мгновенного списания данных.
* **Broker_Events_Contract.yaml:**
	* Добавлена схема сообщения для брокера с изменением времени, связывающее время в генераторе и на бэкэнде.

## [0.2.4] — 17-11-2026
### 🔄 Изменено
- **Backend_Core_API.yaml:**
	- Все ответы с кодами `400`, `404`, `409`, `500` теперь содержат унифицированную структуру тела ошибки через `$ref: '#/components/schemas/ErrorResponse'`.
	- Добавлена схема `ErrorResponse` в `components/schemas` с полями `error_code`, `message`, `timestamp`, `trace_id`, `details`.
	- Обновлены существующие эндпоинты групп (`GET /api/v1/groups`, `POST /api/v1/groups`, `GET /api/v1/groups/{group_id}`):
		- Тег изменён с «Пользователи и Группы» на «Семейные группы».
		- Добавлены ответы `401` и `403` с телом `ErrorResponse`.
		- В `POST /api/v1/groups` сохранено поле `owner_id` в теле запроса (для обратной совместимости) + добавлена JWT-аутентификация.
- **Generator_Service_API.yaml:**
	- Все ответы с кодами `400`, `409`, `500` во всех эндпоинтах секции «События» и «Управление временем» теперь содержат унифицированную структуру тела ошибки через `$ref: '#/components/schemas/ErrorResponse'`.
	- Добавлена схема `ErrorResponse` в `components/schemas` с полями `error_code`, `message`, `timestamp`, `trace_id`, `details`.
- **Data_Dictionary.md**
	- Добавлены новые поля в таблицу AuthCode
- **Generator_Service_API.yaml:**
	- Порт сервера изменён с `8080` на `8082`.
	- В схему `UserBackgroundStopRequest` добавлено обязательное поле `resource_type`.
- **Backend_Core_API.yaml:**
	- Добавлен эндпоинт `GET /api/v1/users/active` для получения списка ID активных пользователей.
	- Добавлена глобальная JWT-аутентификация (`securitySchemes.BearerAuth` + глобальный `security`) для всех эндпоинтов API.
	- Добавлены новые эндпоинты управления семейными группами:
		- `POST /api/v1/groups/{group_id}/members` — добавление участника (требуется роль OWNER).
		- `DELETE /api/v1/groups/{group_id}/members/{user_id}` — удаление участника (требуется роль OWNER).
		- `POST /api/v1/groups/{group_id}/leave` — выход участника из группы.
		- `POST /api/v1/groups/{group_id}/transfer-ownership` — передача прав владельца (требуется роль OWNER).
		- `POST /api/v1/groups/{group_id}/disband` — роспуск группы (требуется роль OWNER).
	- Добавлены новые схемы запросов: `AddMemberRequest`, `LeaveGroupRequest`, `TransferOwnershipRequest`.
	- Добавлены новые коды ответов во все эндпоинты групп: `401` (неавторизован), `403` (нет прав), `204` (успех без тела).
- **Generator_Service_API.yaml:**
	- Добавлен эндпоинт `POST /api/v1/generator/events/instant-deduct` для мгновенного списания ресурсов.
	- Добавлен эндпоинт `POST /api/v1/generator/events/delayed` для отправки запоздалых событий.
	- Добавлен эндпоинт `GET /api/v1/generator/events/background/status` для получения статуса всех активных генераций.
	- Добавлены схемы: `InstantDeductRequest`, `DelayedEventRequest`, `GenerationStatusResponse`, `TaskInfo`.

## [0.3.0] — 19-11-2026
### ➕ Добавлено
- **Mobile_App_API.yaml:**
	* Создан новый API-контракт для мобильного приложения в `04_API/Mobile_App_API.yaml`.
	* Описаны 6 эндпоинтов модуля «Авторизация и Профиль»:
		- `POST /api/v1/auth/request-code` — запрос кода подтверждения с авторегистрацией.
		- `POST /api/v1/auth/verify-code` — проверка кода, выдача токенов.
		- `POST /api/v1/auth/refresh` — обновление пары токенов с ротацией.
		- `POST /api/v1/auth/logout` — завершение сессии (идемпотентный).
		- `GET /api/v1/users/me` — получение профиля текущего пользователя.
		- `PATCH /api/v1/users/me` — обновление имени (онбординг).
	- Описаны 10 эндпоинтов модуля «Семейные группы»:
		- `GET /api/v1/groups/me` — получение активной группы текущего пользователя.
		- `POST /api/v1/groups` — создание семейной группы.
		- `GET /api/v1/contacts` — список контактов для приглашения.
		- `POST /api/v1/groups/{group_id}/members` — добавление участника в группу.
		- `PUT /api/v1/groups/{group_id}/members/{user_id}/limits` — установка/изменение индивидуального лимита участника.
		- `POST /api/v1/groups/{group_id}/members/{user_id}/exclude` — исключение участника из группы.
		- `POST /api/v1/groups/{group_id}/leave` — выход участника из группы.
		- `POST /api/v1/groups/{group_id}/transfer` — инициирование передачи прав Владельца.
		- `POST /api/v1/groups/{group_id}/transfer/accept` — принятие прав Владельца.
		- `POST /api/v1/groups/{group_id}/disband` — расформирование группы Владельцем.
	- Описаны 3 эндпоинта модуля «Статистика, Прогнозы и История»:
		- `GET /api/v1/groups/{group_id}/statistics` — получение статистики потребления по дням (с ролевой фильтрацией OWNER/MEMBER, поддержкой архивных периодов и фильтра по типу ресурса).
		- `GET /api/v1/groups/{group_id}/forecast` — расчёт прогнозной даты исчерпания общего пакета с флагом надёжности `is_reliable` (минимум 3 дня данных).
		- `GET /api/v1/groups/{group_id}/history` — получение пагинрованной истории изменений состава и лимитов группы (AuditLog) с ролевой фильтрацией.
	* Добавлены схемы: `RequestCodeRequest`, `RequestCodeResponse`, `VerifyCodeRequest`, `VerifyCodeResponse`, `RefreshRequest`, `RefreshResponse`, `LogoutRequest`, `UserProfileResponse`, `UserProfile`, `UpdateUserRequest`, `GroupContext`, `TokenPair`, `ErrorResponse`, `GroupDetailResponse`, `GroupMemberDetail`, `UserLimitDetail`, `PackageInfo`, `TransferPendingInfo`, `TransferPendingResponse`, `ContactsResponse`, `Contact`, `UserLimitResponse`, StatisticsResponse`, `MemberUsageStat`, `DailyUsageEntry`, `ForecastResponse`, `HistoryResponse`, `AuditLogEntry`, `PaginationInfo`.
	* Реализован единый формат ошибок.
	- Перечисление `action_type` в `AuditLogEntry` расширено полным набором значений из Data_Dictionary (`ADD_MEMBER`, `REMOVE_MEMBER`, `CHANGE_LIMIT`, `TRANSFER_OWNERSHIP`, `DISBAND_GROUP`, `GROUP_CREATED`, `MEMBER_ADDED`, `MEMBER_EXCLUDED`, `MEMBER_LEFT`, `LIMIT_CHANGED`, `OWNERSHIP_TRANSFERRED`, `GROUP_DISBANDED`).
### 🔄 Изменено
- **Business_Requirements.md:**
	- **BR-06** актуализирован: алгоритм расчёта предполагаемой даты исчерпания применяется теперь независимо к обоим типам ресурсов — трафику и минутам (остаток ресурса делится на сумму среднесуточного потребления по соответствующему типу).
- **Data_Dictionary.md:**
	- В таблице **User** поле `name` явно помечено как `Nullable` (для новых пользователей до онбординга), добавлено ограничение длины 2–50 символов.
	- В таблице **AuthCode** добавлены поля:
		- `session_id` (UUID, unique) — идентификатор сессии авторизации между request-code и verify-code.
		- `requested_at` — время последнего запроса кода для проверки кулдауна (60 секунд).
	- Уточнены комментарии к полям `attempts_count` и `blocked_until`.