---
layout: default
title: DROP VIEW
nav_order: 22
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# DROP VIEW

Запрос позволяет удалить [логическое представление](../../../overview/main_concepts/logical_view/logical_view.md) 
из [логической базы данных](../../../overview/main_concepts/logical_db/logical_db.md).

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

При успешном выполнении запроса логическое представление удаляется из 
[логической схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md).

## Синтаксис {#syntax}

```sql
DROP VIEW [db_name.]view_name
```

## Параметры {#parameters}

*   `db_name` — имя логической базы данных, из которой удаляется логическое представление. Указывается 
    опционально, если выбрана логическая БД, [используемая по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md);
*   `view_name` — имя удаляемого логического представления.

## Пример {#examples}

```sql
DROP VIEW sales.stores_by_sold_products
```