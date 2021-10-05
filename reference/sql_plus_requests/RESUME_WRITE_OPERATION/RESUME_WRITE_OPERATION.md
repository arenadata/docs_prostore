---
layout: default
title: RESUME_WRITE_OPERATION
nav_order: 31
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# RESUME_WRITE_OPERATION

Запрос позволяет возобновить обработку заблокированных 
[операций записи](../../../overview/main_concepts/write_operation/write_operation.md) горячей 
[дельты](../../../overview/main_concepts/delta/delta.md) — повторно загрузить и (или) отменить изменения по этим операциям. 
Под заблокированными понимаются операции записи, которые по каким-либо причинам остались в статусе «Выполняется» или
«Ошибка» и не были завершены или отменены.

Аналогичный процесс возобновления автоматически запускается для всех заблокированных операций при рестарте системы.
{: .note-wrapper}

Перед выполнением запроса необходимо определить
[логическую базу данных](../../../overview/main_concepts/logical_db/logical_db.md),
[используемую по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md),
если она еще не определена.

Возобновить обработку можно для одной или всех заблокированных операций горячей дельты. Если заблокированная операция 
имеет статус «Ошибка», то изменения, внесенные этой операцией, отменяются; если операция имеет статус «Выполняется», то 
изменения повторно загружаются в [СУБД](../../../introduction/supported_DBMS/supported_DBMS.md)
[хранилища](../../../overview/main_concepts/data_storage/data_storage.md).

Возобновление недоступно для заблокированных операций 
[обновления данных](../../../working_with_system/data_update/data_update.md) со статусом «Выполняется». 
Если в горячей дельте есть такие операции, следует отменить все изменения дельты с помощью запроса 
[ROLLBACK DELTA](../../sql_plus_requests/ROLLBACK_DELTA/ROLLBACK_DELTA.md). 
<br>Статусы операций и исходные запросы, которыми были 
запущены эти операции, можно узнать с помощью запроса
[GET_WRITE_OPERATIONS](../../sql_plus_requests/GET_WRITE_OPERATIONS/GET_WRITE_OPERATIONS.md).
{: .note-wrapper}

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

## Синтаксис {#syntax}

Возобновление обработки одной заблокированной операции записи:
```sql
RESUME_WRITE_OPERATION([write_operation_number])
```

Возобновление обработки всех заблокированных операций записи горячей дельты:
```sql
RESUME_WRITE_OPERATION()
```

Параметры:
*   `write_operation_number` — номер операции записи, обработку которой нужно возобновить. Номера и статусы всех операций 
    горячей дельты можно узнать с помощью запроса
    [GET_WRITE_OPERATIONS](../../sql_plus_requests/RESUME_WRITE_OPERATION/RESUME_WRITE_OPERATION.md). Если номер 
    не указан, возобновляется обработка всех заблокированных операций горячей дельты в 
    [логической базе данных](../../../overview/main_concepts/logical_db/logical_db.md).

## Пример {#examples}

```sql
RESUME_WRITE_OPERATION(14)
```