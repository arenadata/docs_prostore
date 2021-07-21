---
layout: default
title: Удаление логической таблицы
nav_order: 4
parent: Управление схемой данных
grand_parent: Работа с системой
has_children: false
---

# Удаление логической таблицы

Чтобы удалить [логическую таблицу](../../../overview/main_concepts/logical_table/logical_table.md) 
и ее данные, выполните запрос [DROP TABLE](../../../reference/sql_plus_requests/DROP_TABLE/DROP_TABLE.md) 
(см. пример [ниже](#пример)).

При успешном выполнении запроса все данные, связанные с логической таблицей, удаляются из указанных 
[СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) [хранилища](../../../overview/main_concepts/data_storage/data_storage.md). 
Логическая таблица удаляется из [логической схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md)
при удалении данных таблицы из последней СУБД (см. секцию `DATASOURCE_TYPE` 
в разделе [DROP TABLE](../../../reference/sql_plus_requests/DROP_TABLE/DROP_TABLE.md)).

Наличие логической таблицы можно проверить, как описано в разделе [Проверка наличия логической таблицы](../entity_presence_check/entity_presence_check.md#проверка-наличия-логической-таблицы).

## Пример

```sql
-- выбор базы данных sales по умолчанию
USE sales

-- удаление таблицы sales из СУБД ADQM
DROP TABLE sales DATASOURCE_TYPE = 'adqm'

-- удаление таблицы sales из оставшихся СУБД (ADB, ADG)
DROP TABLE sales
```