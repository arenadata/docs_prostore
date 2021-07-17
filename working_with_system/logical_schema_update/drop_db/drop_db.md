---
layout: default
title: Удаление логической базы данных
nav_order: 2
parent: Управление схемой данных
grand_parent: Работа с системой
has_children: false
---

# Удаление логической базы данных

Чтобы удалить [логическую базу данных](../../../overview/main_concepts/logical_db/logical_db.md), 
выполните запрос [DROP DATABASE](../../../reference/sql_plus_requests/DROP_DATABASE/DROP_DATABASE.md) 
(см. пример [ниже](#пример)).

При успешном выполнении запроса все данные, связанные с логической БД, удаляются из хранилища, и 
все соответствующие логические сущности удаляются из [схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md).

Наличие логической базы данных можно проверить, как описано в разделе [Проверка наличия логической базы данных](../entity_presence_check/entity_presence_check.md#проверка-наличия-логической-базы-данных).

## Пример

``` sql
-- удаление логической базы данных sales
DROP DATABASE sales
```