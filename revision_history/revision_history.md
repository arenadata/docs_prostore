---
layout: default
title: История изменений
nav_order: 7
has_children: false
has_toc: false
---

# История изменений

## Текущая версия документации (4.1)

Изменения:
* добавлено ключевое слово `OFFSET`, 
  [доступное в SELECT-запросах](../reference/sql_plus_requests/SELECT/SELECT.md#ключевое-слово-offset);
* добавлено ключевое слово `FETCH NEXT <N> ROWS ONLY` как полная альтернатива ключевому слову `LIMIT <N>` 
  в [SELECT](../reference/sql_plus_requests/SELECT/SELECT.md)-запросах;
* обновлено описание запроса [ROLLBACK DELTA](../reference/sql_plus_requests/ROLLBACK_DELTA/ROLLBACK_DELTA.md): 
  теперь запрос отменяет как завершенные, так и выполняемые операции записи;
* обновлена [конфигурация](../maintenance/configuration/configuration.md):
  * значение параметра `ADB_EXECUTORS_COUNT` изменено с 20 на 3;
  * значение параметра `ADB_MAX_POOL_SIZE` изменено с 5 на 3;
  * добавлен новый параметр `DELTA_ROLLBACK_STATUS_CALLS_MS`.

## Архивные версии документации

Версия 4.0

Изменения:
* описаны [материализованные представления](../overview/main_concepts/materialized_view/materialized_view.md);
* описаны возможные [форматы даты и времени в запросах](../reference/timestamp_formats/timestamp_formats.md);
* добавлен раздел [Проверка наличия логической сущности]((../working_with_system/logical_schema_update/entity_presence_check/entity_presence_check.md));
* добавлен раздел [Настройка JSON-логов](../maintenance/json_logs_configuration/json_logs_configuration.md);
* в [конфигурацию](../maintenance/configuration/configuration.md) добавлены параметры для управления материализованными 
  представлениями:
  * `MATERIALIZED_VIEWS_SYNC_PERIOD_MS`,
  * `MATERIALIZED_VIEWS_RETRY_COUNT`,
  * `MATERIALIZED_VIEWS_RETRY_COUNT`.

[Версия 3.7.3](https://arenadata.github.io/docs_prostore_archive/v3-7-3/)

Изменения:
* обновлена конфигурация:
  * в секцию `vertx.pool` добавлены параметры `DTM_CORE_WORKER_POOL_SIZE` и `DTM_CORE_EVENT_LOOP_POOL_SIZE`;
  * путь к параметру `ADB_MAX_POOL_SIZE` изменился с `adb.maxSize` на `adb.poolSize`;
  * в секцию `adb` добавлен параметр `ADB_EXECUTORS_COUNT`;
* описан запрос ROLLBACK CRASHED_WRITE_OPERATIONS;
* доработаны разделы CHECK_DATA и CHECK_SUM: описаны алгоритм и пример расчета контрольной суммы;
* уточнено описание формата загрузки и формата выгрузки данных;
* в разделе «Минимальные системные требования» версия ADG обновлена до 2.7.2.