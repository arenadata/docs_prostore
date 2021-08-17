---
layout: default
title: Удаление материализованного представления
nav_order: 9
parent: Управление схемой данных
grand_parent: Работа с системой
has_children: false
---

# Удаление материализованного представления {#drop_materialized_view}

Чтобы удалить [материализованное представление](../../../overview/main_concepts/materialized_view/materialized_view.md) 
и его данные, выполните запрос [DROP MATERIALIZED VIEW](../../../reference/sql_plus_requests/DROP_MATERIALIZED_VIEW/DROP_MATERIALIZED_VIEW.md) 
(см. примеры [ниже](#examples)):
* без ключевого слова `LOGICAL_ONLY` — чтобы удалить материализованное представление 
  из [логической схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md), а также
  связанные [физические таблицы](../../../overview/main_concepts/physical_table/physical_table.md) и размещенные
  в них данные — из [хранилища данных](../../../overview/main_concepts/data_storage/data_storage.md);
* с ключевым словом `LOGICAL_ONLY` — чтобы удалить материализованное представление только на логическом уровне
  (из логической схемы данных). Удаление сущностей только на логическом уровне может быть полезно, например,
  при изменении физической и логических схем данных.

Если данные материализованного представления нужно удалить только из некоторых [СУБД](../../../introduction/supported_DBMS/supported_DBMS.md)
хранилища, добавьте в запрос ключевое слово `DATASOURCE_TYPE` с псевдонимами требуемых СУБД
(см. пример [ниже](#adg_example)). По умолчанию, если ключевое слово не указано в запросе, данные
представления удаляются изо всех СУБД хранилища. Само материализованное представление удаляется из 
логической схемы данных при удалении его данных из последней СУБД хранилища.

Наличие материализованного представления можно проверить, как описано в разделе
[Проверка наличия материализованного представления](../entity_presence_check/entity_presence_check.md#mat_view_check).
Наличие физических таблиц, связанных с материализованным представлением, можно проверить, как описано в разделе 
[Проверка месторасположения логической сущности](../../../working_with_system/other_features/datasource_check/datasource_check.md).

## Примеры {#examples}

### Удаление материализованного представления из одной СУБД {#adg_example}
```sql
-- выбор базы данных sales по умолчанию
USE sales

-- удаление представления sales_and_stores
DROP MATERIALIZED VIEW sales_and_stores DATASOURCE_TYPE = 'adg'
```

### Удаление материализованного представления из всех СУБД {#all_db_example}

```sql
DROP MATERIALIZED VIEW sales.sales_and_stores
```

### Удаление материализованного представления только на логическом уровне {#logical_example}

```sql
DROP MATERIALIZED VIEW sales.stores_by_sold_products_matview LOGICAL_ONLY
```