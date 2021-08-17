---
layout: default
title: DROP TABLE
nav_order: 19
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# DROP TABLE
{: .no_toc }

<details markdown="block">
  <summary>
    Содержание раздела
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

Запрос позволяет удалить [логическую таблицу](../../../overview/main_concepts/logical_table/logical_table.md) 
и ее данные. 

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

Для удаления данных логических таблиц только из некоторых [СУБД](../../../introduction/supported_DBMS/supported_DBMS.md)
[хранилища](../../../overview/main_concepts/data_storage/data_storage.md) можно указать
ключевое слово `DATASOURCE_TYPE` (см. секцию [Ключевое слово DATASOURCE_TYPE](#datasource_type)).

**Внимание:** удаленные данные невозможно восстановить средствами системы. Удаление данных из СУБД хранилища, 
используемых для выгрузки данных, приведет к невозможности [выгрузки](../../../working_with_system/data_download/data_download.md) 
этих данных. Подробнее о СУБД, из которых можно выгружать данные, см. в разделе 
[INSERT INTO download_external_table](../INSERT_INTO_download_external_table/INSERT_INTO_download_external_table.md).

## Синтаксис {#syntax}

```sql
DROP TABLE [IF EXISTS] [db_name.]table_name
[DATASOURCE_TYPE = datasource_alias]
[LOGICAL_ONLY]
```

Где: 
*   `db_name` — имя логической базы данных, из которой удаляется логическая таблица. Указывается 
    опционально, если выбрана логическая БД, [используемая по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md);
*   `table_name` — имя удаляемой логической таблицы;
*   `datasource_alias` — псевдоним СУБД хранилища, из которой удаляются данные логической таблицы. 
    Возможные значения: `adb`, `adqm`, `adg`, `adp`. Значение можно указывать без кавычек, в одинарных кавычках 
    (например, `'adb'`) или двойных кавычках (например, `"adb"`). Если ключевое слово `DATASOURCE_TYPE` 
    с псевдонимом не указано, данные удаляются из всех СУБД хранилища.

### Ключевое слово IF EXISTS {#if_exists}

Ключевое слово `IF EXISTS` включает проверку наличия логической таблицы до попытки ее удаления.
Если ключевое слово указано в запросе, система возвращает успешный ответ как по успешно
удаленной, так и несуществующей таблице; иначе, если ключевое слово не указано, — только
по успешно удаленной таблице.

### Ключевое слово DATASOURCE_TYPE {#datasource_type}

Ключевое слово `DATASOURCE_TYPE` позволяет указать СУБД хранилища, из которых необходимо
удалить данные логической таблицы. Сама логическая таблица удаляется из
логической схемы данных при удалении данных таблицы из последней СУБД хранилища.

По умолчанию, если ключевое слово не указано, данные таблицы удаляются из всех СУБД хранилища.

### Ключевое слово LOGICAL_ONLY {#logical_only}

Ключевое слово `LOGICAL_ONLY` позволяет удалить логическую таблицу только на логическом уровне
(из [логической схемы данных](../../../overview/main_concepts/logical_schema/logical_schema.md)), без
удаления связанных [физических таблиц](../../../overview/main_concepts/physical_table/physical_table.md)
и размещенных в них данных из хранилища данных. Это может быть полезно, например, при изменении 
физической и логических схем данных.

По умолчанию, если ключевое слово не указано, система удаляет логическую таблицу и связанные 
с ней физические таблицы.
    
## Примеры {#examples}

### Удаление таблицы с удалением данных из всех СУБД {#all_db_example}

```sql
DROP TABLE sales.sales
```

### Удаление таблицы и ее данных с проверкой наличия таблицы {#example_with_check}

```sql
DROP TABLE IF EXISTS sales.sales_unknown_existence
```

### Удаление таблицы с удалением данных из ADB и ADG {#adb_adg_example}

```sql
DROP TABLE sales.stores DATASOURCE_TYPE = adb
DROP TABLE sales.stores DATASOURCE_TYPE = adg
```

### Удаление таблицы только на логическом уровне {#logical_example}

```sql
DROP TABLE sales.sales1 LOGICAL_ONLY
```