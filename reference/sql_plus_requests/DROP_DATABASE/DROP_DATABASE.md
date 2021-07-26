---
layout: default
title: DROP DATABASE
nav_order: 16
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# DROP DATABASE

Запрос позволяет удалить [логическую базу данных](../../../overview/main_concepts/logical_db/logical_db.md) 
и все ее данные.

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

При успешном выполнении логическая база данных удаляется из 
[логической схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md), 
и все связанные с ней данные удаляются из [хранилища](../../../overview/main_concepts/data_storage/data_storage.md).

**Внимание:** при удалении логической базы данных удаляются **все** ее данные и вся история изменений 
данных этой логической БД. Удаленные данные не подлежат восстановлению средствами системы.

## Синтаксис {#syntax}

```sql
DROP DATABASE db_name
```

## Параметры {#parameters}

*   `db_name` — имя удаляемой логической базы данных.

## Ограничения {#restrictions}

Не допускается удаление системной базы данных с именем `INFORMATION_SCHEMA`.

## Пример {#examples}

```sql
DROP DATABASE sales
```