---
layout: default
title: CREATE TABLE
nav_order: 13
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
В зависимости от параметров запроса данные логической таблицы размещаются в указанных или всех 
[СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) 
[хранилища](../../../overview/main_concepts/data_storage/data_storage.md).

Рекомендуется создавать логическую таблицу с размещением данных, как минимум, в СУБД, которая выбрана 
для выгрузки данных в [конфигурации](../../../maintenance/configuration/configuration.md) системы. 
Данные могут размещаться в любых СУБД хранилища, однако, если они размещаются вне СУБД выгрузки, 
их выгрузка недоступна.

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

При успешном выполнении запроса таблица появляется в [логической схеме данных](../../../overview/main_concepts/logical_schema/logical_schema.md). 
Соответствующие [физические таблицы](../../../overview/main_concepts/physical_table/physical_table.md) 
появляются в СУБД хранилища, указанных в запросе, или, если СУБД не указаны в запросе, — 
во всех СУБД хранилища.

**Примечание:** изменение логической таблицы недоступно. Для замены некорректной таблицы необходимо 
удалить ее и создать новую.

## Синтаксис {#syntax}

```sql
CREATE TABLE [db_name.]table_name (
  column_name_1 datatype_1 NOT NULL,
  column_name_2 datatype_2 DEFAULT default_value_2,
  column_name_3 datatype_3,
  PRIMARY KEY (column_list_1)
) DISTRIBUTED BY (column_list_2)
[DATASOURCE_TYPE (datasource_aliases)]
```

## Параметры {#parameters}

*   `db_name` — имя логической базы данных, в которой создается логическая таблица. Указывается 
    опционально, если выбрана логическая БД, [используемая по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md);
*   `table_name` — имя создаваемой логической таблицы, уникальное среди логических сущностей логической БД;
*   `column_name_N` — имя столбца таблицы;
*   `datatype_N` — тип данных столбца `column_name_N`. Возможные значения см. 
    в разделе [Логические типы данных](../../supported_data_types/logical_data_types/logical_data_types.md);
*   `default_value_N` — значение столбца `column_name_N` по умолчанию;
*   `column_list_1` — список столбцов, входящих в первичный ключ таблицы;
*   `column_list_2` — список столбцов целочисленного типа, входящих в ключ шардирования таблицы. Столбцы 
    должны быть из числа столбцов `column_list_1`;
*   `datasource_aliases` — список псевдонимов СУБД хранилища, в которых нужно разместить данные таблицы. 
    Элементы списка перечисляются через запятую. Возможные значения: `adb`, `adqm`, `adg`.
    Значения можно указывать без кавычек (например, `adb`) или двойных кавычках (например, `"adb"`).

## Ограничения {#restrictions}

*   Имена столбцов должны быть уникальны в рамках логической таблицы.
*   Недопустимо использование зарезервированных имен столбцов: `sys_op`, `sys_from`, `sys_to`, 
    `sys_close_date`, `bucket_id`, `sign`.
*   Первичный ключ должен включать все столбцы ключа шардирования.
*   Ключ шардирования может содержать только целочисленные столбцы.

## Примеры {#examples}

### Таблица с размещением данных во всех СУБД хранилища {#example_with_all_dbms}

Создание логической таблицы с размещением данных во всех СУБД хранилища:
```sql
CREATE TABLE sales.sales (
  identification_number INT NOT NULL,
  transaction_date TIMESTAMP NOT NULL,
  product_code VARCHAR(256) NOT NULL,
  product_units INT NOT NULL,
  store_id INT NOT NULL,
  description VARCHAR(256),
  PRIMARY KEY (identification_number)
)
DISTRIBUTED BY (identification_number)
```

### Таблица с составным первичным ключом {#example_with_compound_pk}

Создание логической таблицы с составным первичным ключом и размещением данных во всех СУБД хранилища:
```sql
CREATE TABLE sales.stores (
  identification_number INT NOT NULL,
  category VARCHAR(256) NOT NULL,
  region VARCHAR(256) NOT NULL,
  address VARCHAR(256) NOT NULL,
  description VARCHAR(256),
  PRIMARY KEY (identification_number, region)
)
DISTRIBUTED BY (identification_number)
```

### Таблица с размещением данных в ADQM и ADG {#example_with_adqm_adg}

Создание логической таблицы с размещением данных в ADQM и ADG (без размещения в ADB):
```sql
CREATE TABLE sales.clients (
  identification_number INT NOT NULL,
  first_name VARCHAR(256) NOT NULL,
  last_name VARCHAR(256) NOT NULL,
  patronymic_name VARCHAR(256),
  birth_date DATE,
  PRIMARY KEY (identification_number)
) DISTRIBUTED BY (identification_number)
DATASOURCE_TYPE (adqm,adg)
```