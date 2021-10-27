---
layout: default
title: ALTER VIEW
nav_order: 1
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# ALTER VIEW

Запрос позволяет изменить вид [логического представления](../../../overview/main_concepts/logical_view/logical_view.md) 
в [логической базе данных](../../../overview/main_concepts/logical_db/logical_db.md).

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

Логическое представление можно также изменить с помощью запроса `CREATE OR REPLACE VIEW` 
(см. [CREATE VIEW](../CREATE_VIEW/CREATE_VIEW.md)).
{: .note-wrapper}

## Синтаксис {#syntax}

```sql
ALTER VIEW [db_name.]view_name AS SELECT query
```

Параметры:
*   `db_name` — имя логической базы данных, в которой находится логическое представление. 
    Указывается опционально, если выбрана логическая БД, [используемая по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md);
*   `view_name` — имя изменяемого логического представления;
*   `query` — [SELECT](../SELECT/SELECT.md)\-подзапрос, на основе которого строится новый вид 
    логического представления.

## Ограничения {#restrictions}

*   Выполнение запроса недоступно в сервисной базе данных `INFORMATION_SCHEMA`.
*   В подзапросе `query` не допускается использование:
    *   логических представлений,
    *   [системных представлений](../../system_views/system_views.md) INFORMATION_SCHEMA,
    *   ключевого слова [FOR SYSTEM_TIME](../SELECT/SELECT.md#for_system_time),
    *   ключевого слова [DATASOURCE_TYPE](../SELECT/SELECT.md#param_datasource_type).

## Пример {#examples}

```sql
ALTER VIEW sales.stores_by_sold_products AS
  SELECT store_id, SUM(product_units) AS product_amount
  FROM sales.sales
  GROUP BY store_id
  ORDER BY product_amount ASC
LIMIT 20
```