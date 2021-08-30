---
layout: default
title: ROLLBACK CRASHED_WRITE_OPERATIONS
nav_order: 28
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# ROLLBACK CRASHED_WRITE_OPERATIONS

Запрос позволяет перезапустить отмену неуспешных [операций записи](../../../overview/main_concepts/write_operation/write_operation.md). 

При ошибке загрузки данных система автоматически отменяет неуспешную операцию записи и возвращает данные СУБД хранилища в состояние, которое предшествовало загрузке. 
Однако, если при откате неуспешной операции записи произошла ошибка (например, из-за неполадок в сети или СУБД), система возвращает исключение и запись не отменяется. 
В этом случае необходимо устранить причины ошибки и вручную перезапустить отмену неуспешных операций записей.

Перед выполнением запроса необходимо определить логическую базу данных, [используемую по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md), 
если она еще не определена. После выполнения запроса необходимо вызвать [ROLLBACK DELTA](../ROLLBACK_DELTA/ROLLBACK_DELTA.md).

В ответе возвращается:
*   объект ResultSet с пустой записью, если неуспешные операции записи отсутствуют;
*   объект ResultSet c записями, каждая из которых содержит имя [логической таблицы](../../../overview/main_concepts/logical_table/logical_table.md) в столбце `table_name`
    и номер отмененной операции записи в столбце `sys_cn_operations`.

## Синтаксис {#syntax}

```sql
ROLLBACK CRASHED_WRITE_OPERATIONS
```