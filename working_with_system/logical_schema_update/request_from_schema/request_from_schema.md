---
layout: default
title: Запрос метаданных логической схемы
nav_order: 15
parent: Управление схемой данных
grand_parent: Работа с системой
has_children: false
---

# Запрос метаданных логической схемы

Чтобы запросить метаданные объектов [логической схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md), 
выполните запрос [SELECT FROM INFORMATION_SCHEMA](../../../reference/sql_plus_requests/SELECT_FROM_INFORMATION_SCHEMA/SELECT_FROM_INFORMATION_SCHEMA.md) 
(см. примеры [ниже](#примеры)). Доступно получение информации о сущностях и их свойствах, перечисленных 
в разделе [Системные представления (INFORMATION_SCHEMA)](../../../reference/system_views/system_views.md).

## Примеры

Запрос списка всех [логических БД](../../../overview/main_concepts/logical_db/logical_db.md) 
[окружения](../../../overview/main_concepts/environment/environment.md) 
с лексической сортировкой по возрастанию:
```sql
SELECT schema_name FROM INFORMATION_SCHEMA.schemata
ORDER BY schema_name
```

Запрос информации о логических сущностях логической БД `SALES`:
```sql
SELECT * FROM INFORMATION_SCHEMA.tables
WHERE table_schema = 'SALES'
```

Запрос списка имен, типов и столбцов логических сущностей окружения:
```sql
SELECT TC.table_schema, TC.table_name, TT.table_type, TC.column_name
FROM information_schema.columns AS TC
JOIN information_schema.tables AS TT
  ON TC.table_schema = TT.table_schema and TC.table_name = TT.table_name
ORDER BY TC.table_schema, TC.table_name
```