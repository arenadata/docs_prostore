---
layout: default
title: CREATE MATERIALIZED VIEW
nav_order: 12
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# CREATE MATERIALIZED VIEW

Запрос позволяет создать [материализованное представление](../../../overview/main_concepts/materialized_view/materialized_view.md) 
в [логической базе данных](../../../overview/main_concepts/logical_db/logical_db.md). 
В зависимости от параметров запроса данные материализованного представления размещаются в указанных или всех 
[СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) 
[хранилища](../../../overview/main_concepts/data_storage/data_storage.md).

**Примечание:** в текущей версии возможно создание материализованных представлений в ADG на основе данных ADB. 

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

При успешном выполнении запроса материализованное представление появляется в [логической схеме данных](../../../overview/main_concepts/logical_schema/logical_schema.md). 
Соответствующие [физические таблицы](../../../overview/main_concepts/physical_table/physical_table.md) 
появляются в СУБД хранилища, указанных в запросе.

**Примечание:** изменение материализованного представления недоступно. Для замены некорректного
материализованного представления необходимо удалить его и создать новое.

## Синтаксис

```sql
CREATE MATERIALIZED VIEW [db_name.]materialized_view_name (
  column_name_1 datatype_1 NOT NULL,
  column_name_2 datatype_2 DEFAULT default_value_2,
  column_name_3 datatype_3,
  PRIMARY KEY (column_list_1)
) DISTRIBUTED BY (column_list_2)
DATASOURCE_TYPE (datasource_aliases)
AS SELECT query
DATASOURCE_TYPE = origin_datasource_alias
```

## Параметры

*   `db_name` — имя логической базы данных, в которой создается материализованное представление. Параметр 
    опционален, если выбрана логическая БД, [используемая по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md);
*   `materialized_view_name` — имя создаваемого логического представления, уникальное среди логических 
    сущностей логической БД;
*   `column_name_N` — имя столбца представления;
*   `datatype_N` — тип данных столбца `column_name_N`. Возможные значения см. 
    в разделе [Логические типы данных](../../supported_data_types/logical_data_types/logical_data_types.md);
*   `default_value_N` — значение столбца `column_name_N` по умолчанию;
*   `column_list_1` — список столбцов, входящих в первичный ключ представления;
*   `column_list_2` — список столбцов, входящих в ключ шардирования представления. 
    Столбцы должны быть из числа столбцов `column_list_1`;
*   `datasource_aliases` — список псевдонимов СУБД хранилища, в которых нужно разместить данные представления. 
    Элементы списка перечисляются через запятую. Возможные значения: `adg`.
    Значения можно указывать без кавычек (например, `adg`) или двойных кавычках (например, `"adg"`);
*   `query` — [SELECT](../SELECT/SELECT.md)-подзапрос, на основе которого строится представление;
*   `origin_datasource_alias` — псевдоним СУБД, которая служит источником данных. 
    Возможные значения: `adb`. Значение указывается в одинарных кавычках (например, `'adb'`).
    
## Ограничения

*   Имена столбцов должны быть уникальны в рамках представления.
*   Столбцы не могут иметь имена, зарезервированные для служебного использования: `sys_op`, `sys_from`, 
    `sys_to`, `sys_close_date`, `bucket_id`, `sign`.
*   Имена, порядок и типы данных столбцов должны совпадать в SELECT-подзапросе и представлении.
*   Первичный ключ должен включать все столбцы ключа шардирования.
*   Подзапрос может обращаться только к [логическим таблицам](../../../overview/main_concepts/logical_table/logical_table.md) 
    и только той логической базы данных, которой принадлежит материализованное представление.
*   Подзапрос не может содержать следующие элементы: 
    * директиву [FOR SYSTEM_TIME](../SELECT/SELECT.md#sect_for_system_time),
    * оператор `ORDER BY`.

## Примеры

Создание материализованного представления на основе одной логической таблицы (`sales`) с размещением данных 
в ADG:

```sql
CREATE MATERIALIZED VIEW sales.sales_december_2020 (
identification_number INT NOT NULL,
transaction_date TIMESTAMP NOT NULL,
product_code VARCHAR(256) NOT NULL,
product_units INT NOT NULL,
store_id INT NOT NULL,
description VARCHAR(256),
PRIMARY KEY (identification_number)
)
DISTRIBUTED BY (identification_number)
DATASOURCE_TYPE (adg)
AS SELECT * FROM sales.sales
   WHERE transaction_date BETWEEN '2020-12-01' AND '2020-12-31'
DATASOURCE_TYPE = 'adb'
```

Создание материализованного представления на основе одной логической таблицы (`sales`) с агрегацией и группировкой:
```sql
CREATE MATERIALIZED VIEW sales.sales_by_stores (
store_id INT NOT NULL,
product_code VARCHAR(256) NOT NULL,
product_units INT NOT NULL,
PRIMARY KEY (store_id, product_code)
)
DISTRIBUTED BY (store_id)
DATASOURCE_TYPE (adg)
AS SELECT store_id, product_code, SUM(product_units) as product_units FROM sales.sales
   WHERE product_code <> 'ABC0001'
   GROUP BY store_id, product_code
DATASOURCE_TYPE = 'adb'
```

Создание материализованного представления на основе соединения двух логических таблиц (`sales` и `stores`):
```sql
CREATE MATERIALIZED VIEW sales.sales_and_stores (
  identification_number INT NOT NULL,
  transaction_date TIMESTAMP NOT NULL,
  product_code VARCHAR(256) NOT NULL,
  product_units INT NOT NULL,
  description VARCHAR(256),
  store_id INT NOT NULL,
  store_category VARCHAR(256) NOT NULL,
  region VARCHAR(256) NOT NULL,
  PRIMARY KEY (identification_number, region)
)
DISTRIBUTED BY (identification_number)
DATASOURCE_TYPE (adg)
AS SELECT
 s.identification_number, s.transaction_date, s.product_code, s.product_units, s.description,
 st.identification_number AS store_id, st.category as store_category, st.region
 FROM sales.sales AS s
 JOIN sales.stores AS st
 ON s.store_id = st.identification_number
DATASOURCE_TYPE = 'adb'
```