---
layout: default
title: Удаление логической таблицы
nav_order: 4
parent: Управление схемой данных
grand_parent: Работа с системой
has_children: false
---

# Удаление логической таблицы {#drop_table}

Чтобы удалить [логическую таблицу](../../../overview/main_concepts/logical_table/logical_table.md) 
и ее данные, выполните запрос [DROP TABLE](../../../reference/sql_plus_requests/DROP_TABLE/DROP_TABLE.md) 
(см. пример [ниже](#examples)):
* без ключевого слова `LOGICAL_ONLY` — чтобы удалить логическую таблицу
  из [логической схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md), а также 
  связанные [физические таблицы](../../../overview/main_concepts/physical_table/physical_table.md) и размещенные 
  в них данные — из [хранилища данных](../../../overview/main_concepts/data_storage/data_storage.md);
* с ключевым словом `LOGICAL_ONLY` — чтобы удалить логическую таблицу только на логическом уровне
  (из логической схемы данных). Удаление сущностей только на логическом уровне может быть полезно, например,
  при изменении физической и логических схем данных.
  
Если данные логической таблицы нужно удалить только из некоторых [СУБД](../../../introduction/supported_DBMS/supported_DBMS.md)
хранилища, добавьте в запрос ключевое слово `DATASOURCE_TYPE` с псевдонимами требуемых СУБД
(см. пример [ниже](#adqm_db_example)). По умолчанию, если ключевое слово не указано в запросе, данные 
таблицы удаляются из всех СУБД хранилища. Сама логическая таблица удаляется из логической схемы данных 
при удалении данных таблицы из последней СУБД хранилища.

Наличие логической таблицы можно проверить, как описано в разделе 
[Проверка наличия логической таблицы](../entity_presence_check/entity_presence_check.md#table_check). Наличие
физических таблиц, связанных с логической, можно проверить, как описано в разделе 
[Проверка месторасположения логической сущности](../../../working_with_system/other_features/datasource_check/datasource_check.md).

## Примеры {#examples}

### Удаление логической таблицы из одной СУБД {#adqm_example}

```sql
-- выбор базы данных sales по умолчанию
USE sales

-- удаление таблицы sales из СУБД ADQM
DROP TABLE sales DATASOURCE_TYPE = 'adqm'
```

### Удаление логической таблицы из всех СУБД {#all_db_example}

```sql
DROP TABLE sales.sales
```

### Удаление логической таблицы только на логическом уровне {#logical_deleting_table_example}

```sql
DROP TABLE sales.sales1 LOGICAL_ONLY
```