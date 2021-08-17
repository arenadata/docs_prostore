---
layout: default
title: Удаление логической базы данных
nav_order: 2
parent: Управление схемой данных
grand_parent: Работа с системой
has_children: false
---

# Удаление логической базы данных {#drop_db}

Чтобы удалить [логическую базу данных](../../../overview/main_concepts/logical_db/logical_db.md) и ее данные, 
выполните запрос [DROP DATABASE](../../../reference/sql_plus_requests/DROP_DATABASE/DROP_DATABASE.md) 
(см. пример [ниже](#examples)):
* без ключевого слова `LOGICAL_ONLY` — чтобы удалить логическую базу данных и все ее дочерние логические сущности 
  из [логической схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md), а также 
  связанную физическую базу данных и размещенные в ней данные — из 
  [хранилища данных](../../../overview/main_concepts/data_storage/data_storage.md);
* с ключевым словом `LOGICAL_ONLY` — чтобы удалить логическую базу данных и все ее дочерние логические сущности 
  только на логическом уровне (из логической схемы данных). Удаление сущностей только на логическом уровне 
  может быть полезно, например, при изменении физической и логических схем данных.

Наличие логической базы данных можно проверить, как описано в разделе 
[Проверка наличия логической базы данных](../entity_presence_check/entity_presence_check.md#db_check).

## Примеры {#examples}

### Удаление логической базы данных {#deleting_db_example}

``` sql
DROP DATABASE sales
```

### Удаление логической базы данных только на логическом уровне {#logical_deleting_db_example}

``` sql
DROP DATABASE sales1 LOGICAL_ONLY
```