---
layout: default
title: CREATE DATABASE
nav_order: 10
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# CREATE DATABASE

Запрос позволяет создать [логическую базу данных](../../../overview/main_concepts/logical_db/logical_db.md) 
в текущем [окружении](../../../overview/main_concepts/environment/environment.md).

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

После успешного выполнения запроса можно создавать [логические таблицы](../../../overview/main_concepts/logical_table/logical_table.md), 
[логические представления](../../../overview/main_concepts/logical_view/logical_view.md) 
и [внешние таблицы](../../../overview/main_concepts/external_table/external_table.md) 
в созданной логической базе данных.

**Совет:** перед наполнением созданной логической базы данных 
[выберите ее в качестве используемой по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md) 
— это позволит не указывать имя логической БД перед именами логических сущностей в запросах к ней.

## Синтаксис {#syntax}

```sql
CREATE DATABASE db_name
```

## Параметры {#parameters}

*   `db_name` — имя создаваемой логической базы данных. Может содержать латинские буквы, цифры и символы подчеркивания (“_”).

## Пример {#examples}

```sql
CREATE DATABASE sales
```