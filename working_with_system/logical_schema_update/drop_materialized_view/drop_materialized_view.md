---
layout: default
title: Удаление материализованного представления
nav_order: 9
parent: Управление схемой данных
grand_parent: Работа с системой
has_children: false
---

# Удаление материализованного представления

Чтобы удалить [материализованное представление](../../../overview/main_concepts/materialized_view/materialized_view.md) 
и его данные, выполните запрос [DROP MATERIALIZED VIEW](../../../reference/sql_plus_requests/DROP_MATERIALIZED_VIEW/DROP_MATERIALIZED_VIEW.md) 
(см. пример [ниже](#пример)).

При успешном выполнении запроса все данные материализованного представления удаляются из указанных 
[СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) [хранилища](../../../overview/main_concepts/data_storage/data_storage.md). 
Материализованное представление удаляется из [логической схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md) 
при удалении данных представления из последней СУБД (см. секцию `DATASOURCE_TYPE` в разделе [DROP MATERIALIZED VIEW](../../../reference/sql_plus_requests/DROP_MATERIALIZED_VIEW/DROP_MATERIALIZED_VIEW.md)).

Наличие материализованного представления можно проверить, как описано в разделе [Проверка наличия материализованного представления](../entity_presence_check/entity_presence_check.md#проверка-наличия-материализованного-представления).

## Пример

```sql
-- выбор базы данных sales по умолчанию
USE sales

-- удаление представления sales_and_stores
DROP MATERIALIZED VIEW sales_and_stores
```