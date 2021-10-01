---
layout: default
title: GET_WRITE_OPERATIONS
nav_order: 28
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# GET_WRITE_OPERATIONS

Запрос позволяет получить информацию обо всех 
[операциях записи](../../../overview/main_concepts/write_operation/write_operation.md) горячей 
[дельты](../../../overview/main_concepts/delta/delta.md). 
Перед выполнением запроса необходимо определить логическую базу данных, 
[используемую по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md), 
если она еще не определена.

По каждой операции доступна следующая информация:
* `sys_cn` — номер операции записи;
* `status` — статус операции записи. Возможные значения: 0 — выполняется, 1 — успешно завершена, 
  2 — ошибка (операция откатывается); 3 — откачена;
* `destination_table_name` — имя 
  [логической таблицы](../../../overview/main_concepts/logical_table/logical_table.md), в которую записывались 
  или записываются данные;
* `external_table_name` — имя [внешней таблицы](../../../overview/main_concepts/external_table/external_table.md) 
  загрузки. Отображается, если операция была запущена 
  [загрузкой данных](../../../working_with_system/data_upload/data_upload.md);
* `query` — запрос, который запустил операцию записи.

В ответе возвращается:
*   объект ResultSet, где каждая запись содержит информацию об одной операции, при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

## Синтаксис {#syntax}

```sql
GET_WRITE_OPERATIONS()
```

<> <скриншот>