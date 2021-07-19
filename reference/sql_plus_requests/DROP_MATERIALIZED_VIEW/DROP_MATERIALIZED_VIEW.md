---
layout: default
title: DROP MATERIALIZED VIEW
nav_order: 18
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# DROP MATERIALIZED VIEW

Запрос позволяет удалить [материализованное представление](../../../overview/main_concepts/materialized_view/materialized_view.md) 
и его данные из [логической базы данных](../../../overview/main_concepts/logical_db/logical_db.md). 
В зависимости от параметров запроса данные представления удаляются из указанных или всех 
[СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) [хранилища](../../../overview/main_concepts/data_storage/data_storage.md).

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

При успешном выполнении запроса данные материализованного представления удаляются из указанных СУБД хранилища. 
Материализованное представление удаляется из [логической схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md) 
при удалении данных из последней СУБД.

## Синтаксис

```sql
DROP MATERIALIZED VIEW [IF EXISTS] [db_name.]materialized_view_name
[DATASOURCE_TYPE = datasource_alias]
```

Опциональная секция `IF EXISTS` включает проверку наличия материализованного представления до 
попытки его удаления. Если эта секция указана в запросе, система возвращает успешный ответ как по успешно
удаленному представлению, так и по несуществующему представлению; иначе, если секция не указана — только 
по успешно удаленному представлению.

## Параметры

*   `db_name` — имя логической базы данных, из которой удаляется материализованное представление. 
    Параметр опционален, если выбрана логическая БД, [используемая по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md);
*   `table_name` — имя удаляемого материализованного представления;
*   `datasource_alias` — псевдоним СУБД хранилища, из которой удаляются данные материализованного представления. 
    Возможные значения: `adg`. Значение можно указывать без кавычек, в одинарных кавычках 
    (например, `'adg'`) или двойных кавычках (например, `"adg"`). Если секция `DATASOURCE_TYPE` 
    с псевдонимом не указана, данные удаляются из всех СУБД хранилища.
    
## Примеры

Удаление материализованного представления с удалением данных из всех СУБД хранилища:
```sql
DROP MATERIALIZED VIEW sales.sales_and_stores
```

Удаление материализованного представления с проверкой его наличия:
```sql
DROP MATERIALIZED VIEW IF EXISTS sales.mat_view_with_unknown_existence
```

Удаление материализованного представления из ADG:
```sql
DROP MATERIALIZED VIEW sales.sales_and_stores DATASOURCE_TYPE = 'adg'
```