---
layout: default
title: История изменений
nav_order: 7
has_children: false
---

# История изменений {#revision_history}
{: .no_toc }

<details markdown="block">
  <summary>
    Содержание раздела
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Текущая версия документации (5.1) {#current}

Изменения:
* добавлено ключевое слово [ESTIMATE_ONLY](../reference/sql_plus_requests/SELECT/SELECT.md#estimate), 
  доступное в [SELECT](../reference/sql_plus_requests/SELECT/SELECT.md)-запросах;
* добавлено ключевое слово `LOGICAL_ONLY`, доступное в запросах на создание и удаление логической БД, 
  логической таблицы и материализованного представления;
* обновлено описание запросов [CHECK_DATA](../reference/sql_plus_requests/CHECK_DATA/CHECK_DATA.md) и
  [CHECK_SUM](../reference/sql_plus_requests/CHECK_SUM/CHECK_SUM.md): 
  * добавлен коэффициент нормализации, повышающий максимально допустимое количество записей в
    проверяемых дельтах;
  * изменен расчет контрольных сумм: теперь они считаются по дельтам, а не отдельным операциям записи; 
* обновлено описание запроса [CHECK_SUM](../reference/sql_plus_requests/CHECK_SUM/CHECK_SUM.md):
  * изменен расчет контрольной суммы по таблице/представлению: теперь расчет аналогичен тому, который 
    выполняется для CHECK_DATA;
  * изменен расчет контрольной суммы по логической БД: теперь контрольные суммы таблиц складываются, 
    а не проходят дополнительный этап хеширования;
* в [конфигурацию](../maintenance/configuration/configuration.md) добавлен параметр 
  `DTM_VERTX_BLOCKING_STACKTRACE_TIME`;
* в главу «Работа с системой» добавлены разделы 
  [Получение информации о SELECT-запросе](../working_with_system/other_features/query_estimation/query_estimation.md) и 
  [Проверка месторасположения логической сущности](../working_with_system/other_features/datasource_check/datasource_check.md);
* в главу «Эксплуатация» добавлен раздел [Часовые пояса системы и компонентов](../maintenance/time_zones/time_zones.md).
  
## Архивные версии документации {#archive}

### Версия 5.0

Изменения:
* добавлена [СУБД хранилища](../introduction/supported_DBMS/supported_DBMS.md) нового типа — ADP —
  на основе PostgreSQL;
* добавлена выгрузка данных из СУБД хранилища, указанной в запросе
  (см. [INSERT INTO download_external_table](../reference/sql_plus_requests/INSERT_INTO_download_external_table/INSERT_INTO_download_external_table.md));
* в [системное представление tables](../reference/system_views/system_views.md#tables) добавлен столбец
  `table_datasource_type`;
* обновлено описание запроса [CHECK_SUM](../reference/sql_plus_requests/CHECK_SUM/CHECK_SUM.md): теперь запрос
  поддерживает расчет контрольной суммы по материализованному представлению;
* обновлена [конфигурация](../maintenance/configuration/configuration.md):
  * добавлены параметры для управления СУБД ADP;
  * добавлены параметры запроса prepared statement для ADB: `ADB_PREPARED_CACHE_MAX_SIZE`, `ADB_PREPARED_CACHE_SQL_LIMIT`
    и `ADB_PREPARED_CACHE`;
  * значения следующих параметров расширены новой СУБД ADP: `CORE_PLUGINS_ACTIVE`, `DTM_CORE_PLUGINS_RELATIONAL`,
    `DTM_CORE_PLUGINS_ANALYTICAL`, `DTM_CORE_PLUGINS_DICTIONARY`, `DTM_CORE_PLUGINS_UNDEFINED`;
  * добавлен параметр `DTM_LOGGING_LEVEL` для управления уровнем логирования;
  * конкретные IP-адреса заменены на `localhost`;
* добавлен раздел [Схемы развертывания](../maintenance/deployment_diagrams/deployment_diagrams.md).

### Версия 4.1

Версия 4.1 доступна в [архиве](https://arenadata.github.io/docs_prostore_archive/v4-1-0/).

Изменения:
* добавлено ключевое слово `OFFSET`, доступное в SELECT-запросах;
* добавлено ключевое слово `FETCH NEXT <N> ROWS ONLY` как полная альтернатива ключевому слову `LIMIT <N>`
  в SELECT-запросах;
* обновлено описание запроса ROLLBACK DELTA: теперь запрос отменяет как завершенные, так и выполняемые операции записи;
* обновлена конфигурация:
  * значение параметра `ADB_EXECUTORS_COUNT` изменено с 20 на 3;
  * значение параметра `ADB_MAX_POOL_SIZE` изменено с 5 на 3;
  * добавлен новый параметр `DELTA_ROLLBACK_STATUS_CALLS_MS`.

### Версия 4.0

Изменения:
* описаны материализованные представления;
* описаны возможные форматы даты и времени в запросах;
* добавлен раздел «Проверка наличия логической сущности»;
* добавлен раздел «Настройка JSON-логов»;
* в конфигурацию добавлены параметры по управлению материализованными представлениями:
  * `MATERIALIZED_VIEWS_SYNC_PERIOD_MS`,
  * `MATERIALIZED_VIEWS_RETRY_COUNT`,
  * `MATERIALIZED_VIEWS_RETRY_COUNT`.

### Версия 3.7.3

Версия 3.7.3 доступна в [архиве](https://arenadata.github.io/docs_prostore_archive/v3-7-3/).

Изменения:
* обновлена конфигурация:
  * в секцию `vertx.pool` добавлены параметры `DTM_CORE_WORKER_POOL_SIZE` и `DTM_CORE_EVENT_LOOP_POOL_SIZE`;
  * путь к параметру `ADB_MAX_POOL_SIZE` изменился с `adb.maxSize` на `adb.poolSize`;
  * в секцию `adb` добавлен параметр `ADB_EXECUTORS_COUNT`;
* описан запрос ROLLBACK CRASHED_WRITE_OPERATIONS;
* доработаны разделы CHECK_DATA и CHECK_SUM: описаны алгоритм и пример расчета контрольной суммы;
* уточнено описание формата загрузки и формата выгрузки данных;
* в разделе «Минимальные системные требования» версия ADG обновлена до 2.7.2.