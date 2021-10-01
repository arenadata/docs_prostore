﻿---
layout: default
title: RESUME WRITE OPERATION
nav_order: 31
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# RESUME WRITE OPERATION

Запрос позволяет возобновить обработку заблокированных 
[операций записи](../../../overview/main_concepts/write_operation/write_operation.md) горячей 
[дельты](../../../overview/main_concepts/delta/delta.md) — повторить загрузку и (или) откат изменений по этим операциям. 
Под заблокированными понимаются операции записи, которые по каким-либо причинам остались в статусе «Выполняется» или
«Ошибка» и не были завершены или откачены.

Перед выполнением запроса необходимо определить
[логическую базу данных](../../../overview/main_concepts/logical_db/logical_db.md),
[используемую по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md),
если она еще не определена.

Возобновить обработку можно для одной или всех заблокированных операций горячей дельты. Если заблокированная операция 
записи находится в статусе «Выполняется», то повторяется загрузка ее изменений в 
[СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) 
[хранилища](../../../overview/main_concepts/data_storage/data_storage.md); если в статусе
«Ошибка» — то повторяется откат изменений по этой операции. Загрузка повторяется только по тем операциям, которые были 
вызваны [загрузкой данных](../../../working_with_system/data_upload/data_upload.md), а операции, вызванные 
[обновлением данных](../../../working_with_system/data_update/data_update.md), не повторяются. Откат изменений 
повторяется для заблокированных операций записи независимо от их происхождения: как для вызванных загрузкой, 
так и обновлением данных. Действие запроса аналогично процессу восстановления операций, который автоматически 
запускается при рестарте системы.

Номера и статусы операций записи можно узнать с помощью запроса 
[GET_WRITE_OPERATIONS](../../sql_plus_requests/RESUME_WRITE_OPERATION/RESUME_WRITE_OPERATION.md).
{: .tip-wrapper}

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
*   `write_operation_number` — номер операции записи, обработку которой нужно возобновить. Если не указан, 
    возобновляется обработка всех заблокированных операций горячей дельты в 
    [логической базе данных](../../../overview/main_concepts/logical_db/logical_db.md).

## Пример {#examples}

```sql
RESUME_WRITE_OPERATION(14)
```