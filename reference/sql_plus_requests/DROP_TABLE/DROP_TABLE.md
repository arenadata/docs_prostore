---
layout: default
title: DROP TABLE
nav_order: 19
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# DROP TABLE

Запрос позволяет удалить [логическую таблицу](../../../overview/main_concepts/logical_table/logical_table.md) 
и ее данные из [логической базы данных](../../../overview/main_concepts/logical_db/logical_db.md). 
В зависимости от параметров запроса данные таблицы удаляются из указанных или всех 
[СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) [хранилища](../../../overview/main_concepts/data_storage/data_storage.md).

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

При успешном выполнении запроса данные логической таблицы удаляются из указанных СУБД хранилища. 
Логическая таблица удаляется из [логической схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md) 
при удалении данных таблицы из последней СУБД.

**Внимание:** удаленные данные не подлежат восстановлению средствами системы. Удаление данных из СУБД, 
заданной для выгрузки в [конфигурации](../../../maintenance/configuration/configuration.md) системы, 
приведет к невозможности [выгрузки](../../../working_with_system/data_download/data_download.md) 
этих данных.

## Синтаксис {#syntax}

```sql
DROP TABLE [IF EXISTS] [db_name.]table_name
[DATASOURCE_TYPE = datasource_alias]
```

Опциональное ключевое слово `IF EXISTS` включает проверку наличия логической таблицы до попытки ее удаления. 
Если это ключевое слово указано в запросе, система возвращает успешный ответ как по успешно
удаленной таблице, так и по несуществующей таблице; иначе, если ключевое слово не указано — только
по успешно удаленной таблице.

## Параметры {#parameters}

*   `db_name` — имя логической базы данных, из которой удаляется логическая таблица. Указывается 
    опционально, если выбрана логическая БД, [используемая по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md);
*   `table_name` — имя удаляемой логической таблицы;
*   `datasource_alias` — псевдоним СУБД хранилища, из которой удаляются данные логической таблицы. 
    Возможные значения: `adb`, `adqm` и `adg`. Значение можно указывать без кавычек, в одинарных кавычках 
    (например, `'adb'`) или двойных кавычках (например, `"adb"`). Если ключевое слово `DATASOURCE_TYPE` 
    с псевдонимом не указано, данные удаляются из всех СУБД хранилища.
    
## Примеры {#examples}

Удаление логической таблицы с удалением данных из всех СУБД хранилища:
```sql
DROP TABLE sales.sales
```

Удаление логической таблицы с проверкой ее наличия:
```sql
DROP TABLE IF EXISTS sales.sales_unknown_existence
```

Последовательное удаление логической таблицы из ADB и ADG:
```sql
DROP TABLE sales.stores DATASOURCE_TYPE = adb
DROP TABLE sales.stores DATASOURCE_TYPE = adg
```