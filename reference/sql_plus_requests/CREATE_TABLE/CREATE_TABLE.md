﻿---
layout: default
title: CREATE TABLE
nav_order: 14
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# CREATE TABLE
{: .no_toc }

<details markdown="block">
  <summary>
    Содержание раздела
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

Запрос позволяет создать [логическую таблицу](../../../overview/main_concepts/logical_table/logical_table.md) 
в [логической базе данных](../../../overview/main_concepts/logical_db/logical_db.md).

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

Для размещения данных логической таблицы только в некоторых [СУБД](../../../introduction/supported_DBMS/supported_DBMS.md)
[хранилища](../../../overview/main_concepts/data_storage/data_storage.md) можно указать 
ключевое слово `DATASOURCE_TYPE` (см. секцию [Ключевое слово DATASOURCE_TYPE](#datasource_type)).

Рекомендуется создавать логическую таблицу с размещением данных, как минимум, в СУБД хранилища, 
из которой планируется выгрузка данных. Иначе выгрузка данных из таблицы будет недоступна. 
Подробнее о СУБД, из которых можно выгружать данные, см. в разделе 
[INSERT INTO download_external_table](../INSERT_INTO_download_external_table/INSERT_INTO_download_external_table.md).
{: .tip-wrapper}

Изменение логической таблицы недоступно. Для замены таблицы необходимо удалить ее и 
создать новую.
{: .note-wrapper}

## Синтаксис {#syntax}

```sql
CREATE TABLE [db_name.]table_name (
  column_name_1 datatype_1 NOT NULL,
  column_name_2 datatype_2,
  column_name_3 datatype_3,
  PRIMARY KEY (column_list_1)
) DISTRIBUTED BY (column_list_2)
[DATASOURCE_TYPE (datasource_aliases)]
[LOGICAL_ONLY]
```

Параметры:
*   `db_name` — имя логической базы данных, в которой создается логическая таблица. Указывается 
    опционально, если выбрана логическая БД, [используемая по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md);
*   `table_name` — имя создаваемой логической таблицы, уникальное среди логических сущностей логической БД;
*   `column_name_N` — имя столбца таблицы;
*   `datatype_N` — тип данных столбца `column_name_N`. Возможные значения см. 
    в разделе [Логические типы данных](../../supported_data_types/logical_data_types/logical_data_types.md);
*   `column_list_1` — список столбцов, входящих в первичный ключ таблицы;
*   `column_list_2` — список столбцов целочисленного типа, входящих в ключ шардирования таблицы. Столбцы 
    должны быть из числа столбцов `column_list_1`;
*   `datasource_aliases` — список псевдонимов СУБД хранилища, в которых нужно разместить данные таблицы. 
    Элементы списка перечисляются через запятую. Возможные значения: `adb`, `adqm`, `adg`.
    Значения можно указывать без кавычек (например, `adb`) или двойных кавычках (например, `"adb"`).
    
### Ключевое слово DATASOURCE_TYPE {#datasource_type}

Ключевое слово `DATASOURCE_TYPE` позволяет указать СУБД хранилища, в которых необходимо 
размещать данные логической таблицы.

Если ключевое слово не указано, данные таблицы размещаются во всех доступных СУБД хранилища.

### Ключевое слово LOGICAL_ONLY {#logical_only}

Ключевое слово `LOGICAL_ONLY` позволяет создать логическую таблицу только на логическом уровне
(в [логической схеме данных](../../../overview/main_concepts/logical_schema/logical_schema.md)), без
пересоздания связанных [физических таблиц](../../../overview/main_concepts/physical_table/physical_table.md)
в хранилище данных.

Если ключевое слово не указано, создается как логическая, так и связанные с ней физические таблицы.

## Ограничения {#restrictions}

*   Выполнение запроса недоступно в сервисной базе данных `INFORMATION_SCHEMA`.
*   Имена столбцов должны быть уникальны в рамках логической таблицы.
*   Недопустимо использование зарезервированных имен столбцов: `sys_op`, `sys_from`, `sys_to`, 
    `sys_close_date`, `bucket_id`, `sign`.
*   Первичный ключ должен включать все столбцы ключа шардирования.
*   Ключ шардирования может содержать только целочисленные столбцы.

## Примеры {#examples}

### Создание таблицы с размещением данных во всех СУБД хранилища {#example_with_all_dbms}

```sql
CREATE TABLE sales.sales (
  id INT NOT NULL,
  transaction_date TIMESTAMP NOT NULL,
  product_code VARCHAR(256) NOT NULL,
  product_units INT NOT NULL,
  store_id INT NOT NULL,
  description VARCHAR(256),
  PRIMARY KEY (id)
)
DISTRIBUTED BY (id)
```

### Создание таблицы с составным первичным ключом {#example_with_compound_pk}

```sql
CREATE TABLE sales.stores (
  id INT NOT NULL,
  category VARCHAR(256) NOT NULL,
  region VARCHAR(256) NOT NULL,
  address VARCHAR(256) NOT NULL,
  description VARCHAR(256),
  PRIMARY KEY (id, region)
)
DISTRIBUTED BY (id)
```

### Создание таблицы с размещением данных в ADQM и ADG {#example_with_adqm_adg}

```sql
CREATE TABLE sales.clients (
  id INT NOT NULL,
  first_name VARCHAR(256) NOT NULL,
  last_name VARCHAR(256) NOT NULL,
  patronymic_name VARCHAR(256),
  birth_date DATE,
  PRIMARY KEY (id)
) DISTRIBUTED BY (id)
DATASOURCE_TYPE (adqm,adg)
```

### Создание таблицы только на логическом уровне {#logical_example}

```sql
CREATE TABLE sales.sales1 (
  id INT NOT NULL,
  transaction_date TIMESTAMP NOT NULL,
  product_code VARCHAR(256) NOT NULL,
  product_units INT NOT NULL,
  store_id INT NOT NULL,
  description VARCHAR(256),
  PRIMARY KEY (id)
)
DISTRIBUTED BY (id)
LOGICAL_ONLY
```