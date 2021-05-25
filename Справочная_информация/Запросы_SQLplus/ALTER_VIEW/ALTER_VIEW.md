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

Запрос позволяет изменить вид [логического представления](../../../Обзор_понятий_компонентов_и_связей/Основные_понятия/Логическое_представление/Логическое_представление.md) 
в [логической базе данных](../../../Обзор_понятий_компонентов_и_связей/Основные_понятия/Логическая_база_данных/Логическая_база_данных.md).

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

**Примечание:** логическое представление можно также изменить с помощью запроса `CREATE OR REPLACE VIEW` 
(см. [CREATE VIEW](../CREATE_VIEW/CREATE_VIEW.md)).

## Синтаксис

```sql
ALTER VIEW [db_name.]view_name AS SELECT query
```

## Параметры

*   `db_name` — имя логической базы данных, в которой находится логическое представление. 
    Указывается опционально, если выбрана логическая БД, [используемая по умолчанию](../../../Работа_с_системой/Другие_функции/Определение_логической_БД_по_умолчанию/Определение_логической_БД_по_умолчанию.md);
*   `view_name` — имя изменяемого логического представления;
*   `query` — [SELECT](../SELECT/SELECT.md)\-подзапрос, на основе которого строится новый вид 
    логического представления.

## Ограничения

В подзапросе `query` не допускается использование:
*   логических представлений,
*   [системных представлений](../../Системные_представления_INFORMATION_SCHEMA/Системные_представления_INFORMATION_SCHEMA.md) 
    INFORMATION_SCHEMA,
*   директивы [FOR SYSTEM_TIME](../SELECT/SELECT.md#sect_for_system_time),
*   секции [DATASOURCE_TYPE](../SELECT/SELECT.md#param_datasource_type).

## Пример

```sql
ALTER VIEW sales.stores_by_sold_products AS
  SELECT store_id, SUM(product_units) AS product_amount
  FROM sales.sales
  GROUP BY store_id
  ORDER BY product_amount ASC
LIMIT 20
```