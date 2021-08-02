---
layout: default
title: SELECT
nav_order: 30
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# SELECT
{: .no_toc }

<details markdown="block">
  <summary>
    Содержание раздела
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

Запрос позволяет выбрать данные из [логических таблиц](../../../overview/main_concepts/logical_table/logical_table.md), 
[логических представлений](../../../overview/main_concepts/logical_view/logical_view.md) 
и (или) [материализованных представлений](../../../overview/main_concepts/materialized_view/materialized_view.md). 
Запрос можно использовать как самостоятельный запрос для [чтения данных](../../../working_with_system/data_reading/data_reading.md), 
а также в качестве подзапроса в следующих запросах:
*   в [запросе на выгрузку данных](../INSERT_INTO_download_external_table/INSERT_INTO_download_external_table.md),
*   в запросе на [создание](../CREATE_VIEW/CREATE_VIEW.md) или [обновление](../ALTER_VIEW/ALTER_VIEW.md) логического представления,
*   в запросе на [создание материализованного представления](../CREATE_MATERIALIZED_VIEW/CREATE_MATERIALIZED_VIEW.md).

С помощью запроса можно выбрать данные любой из категорий:
*   актуальные данные;
*   архивные данные, которые были актуальны в указанный момент времени;
*   изменения данных, выполненные в рамках указанного диапазона [дельт](../../../overview/main_concepts/delta/delta.md).

Запрос обрабатывается в порядке, описанном в разделе [Порядок обработки запросов на чтение данных](../../../overview/interactions/llr_processing/llr_processing.md).

В ответе возвращается:
*   объект ResultSet c выбранными записями при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

## Синтаксис {#syntax}

```sql
SELECT column_list
FROM [db_name.]entity_name
[FOR SYSTEM_TIME time_expression [AS alias_name]]
[DATASOURCE_TYPE = datasource_alias]
```

Где:
*   `column_list` — список выбираемых столбцов таблицы или представления. Можно указывать символ `*`
    для выбора всех столбцов;
*   `db_name` — имя логической базы данных, из которой выбираются данные. Указывается опционально, если
    выбрана логическая БД, [используемая по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md);
*   `entity_name` — имя таблицы или представления, из которого выбираются данные;
*   `time_expression` — выражение, задающее момент или период времени, за который выбираются
    данные или изменения данных. Синтаксис выражения см. [ниже](#for_system_time);
*   `alias_name` — псевдоним таблицы или представления. Может включать латинские буквы, цифры и символы
    подчеркивания;
*   `datasource_alias` — системный псевдоним СУБД хранилища, из которой выбираются данные. Возможные
    значения: `'adb'`, `'adqm'`, `'adg'`, `'adp'`.

В запросах можно указывать: 
* ключевые слова, перечисленные в секции [Поддерживаемые ключевые слова](#key_words), 
* функции, перечисленные в разделе [Поддержка SQL](../../sql_support/sql_support.md),
* псевдонимы для имен таблиц, представлений и столбцов.

**Примечание:** некоторые агрегатные функции и типы соединений недоступны для исполнения в определенных
СУБД хранилища. Список доступных возможностей см. в разделе [Поддержка SQL](../../sql_support/sql_support.md).

### Поддерживаемые ключевые слова {#key_words}
    
В запросе можно использовать следующие ключевые слова, которые должны быть указаны в порядке их перечисления:
1.   `FOR SYSTEM_TIME` — для указания момента времени или периода, за который выбираются данные или 
    изменения данных. Если ключевое слово не указано, из логической таблицы и логического представления 
    выбираются данные, актуальные на момент обработки запроса, из материализованного представления — данные, 
     актуальные на момент последней синхронизации представления. Описание синтаксиса см. в секции 
     [Ключевое слово FOR SYSTEM_TIME](#for_system_time);
2.   `JOIN ON` — для соединения данных нескольких логических таблиц и (или) представлений из одной или
     нескольких [логических БД](../../../overview/main_concepts/logical_db/logical_db.md). 
     Возможные префиксы см. в секции [Возможные типы соединений (префиксы JOIN)](#join_prefixes);
3.   `WHERE` — для указания условий выбора данных;
4.   `GROUP BY` — для группировки данных;
5.   `HAVING` — для указания условий выбора сгруппированных данных;
6.   `ORDER BY` — для сортировки данных;
7.   `LIMIT` или `FETCH NEXT <N> ROWS ONLY`— для ограничения количества возвращаемых строк;
8.   `OFFSET` — для пропуска указанного количества строк в результате запроса. Описание синтаксиса см. в 
     секции [Ключевое слово OFFSET](#offset);
9.   <a id="param_datasource_type"></a>`DATASOURCE_TYPE` — для указания [СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) 
    [хранилища](../../../overview/main_concepts/data_storage/data_storage.md), 
    из которой выбираются данные.
     
### Ключевое слово FOR SYSTEM_TIME {#for_system_time}

Ключевое слово `FOR SYSTEM_TIME` позволяет указать момент, по состоянию на который запрашиваются данные,
или период (диапазон дельт), за который запрашиваются изменения. Ключевое слово относится к логической таблице,
логическому представлению или материализованному представлению, после имени которого оно следует. Если в запросе
соединяется несколько логических таблиц и представлений, для каждой логической сущности можно указать свое ключевое слово
`FOR SYSTEM_TIME`, при этом значения этих ключевых слов могут различаться (см. пример [ниже](#deltas_example)).

**Примечание:** наличие и значение ключевого слова `FOR SYSTEM_TIME` для материализованного представления
влияют на порядок маршрутизации запроса (см. раздел [Маршрутизация запросов к данным материализованных представлений](../../../working_with_system/data_reading/routing/routing.md#mat_view_routing)).

Ключевое слово указывается в формате `FOR SYSTEM_TIME time_expression`, где выражение `time_expression`
принимает одно из следующих значений:
*   `AS OF 'YYYY-MM-DD hh:mm:ss'` — запрос данных, актуальных на указанную дату и время. Возможные форматы
    даты и времени см. в разделе [Форматы даты и времени в запросах](../../timestamp_formats/timestamp_formats.md);
*   `AS OF DELTA_NUM delta_num` — запрос данных, актуальных на дату и время закрытия дельты
    с номером `delta_num`;
*   `AS OF LATEST_UNCOMMITTED_DELTA` — запрос данных на текущий момент, включая данные, загруженные
    в рамках открытой (горячей) дельты. По горячей дельте возвращаются записи, загруженные в рамках
    непрерывного диапазона завершенных операций записи (см. параметры `cn_from` и `cn_to`
    в разделе [GET_DELTA_HOT](../GET_DELTA_HOT/GET_DELTA_HOT.md));
*   `STARTED IN (delta_num1, delta_num2)` — запрос данных, добавленных или измененных в период
    между дельтой `delta_num1` и дельтой `delta_num2` (включительно);
*   `FINISHED IN (delta_num1, delta_num2)` — запрос данных, удаленных в период между дельтой
    `delta_num1` и дельтой `delta_num2` (включительно).

Следующие значения ключевого слова не поддерживаются в запросах к материализованным представлениям:
* `FOR SYSTEM_TIME AS OF LATEST_UNCOMMITTED_DELTA`;
* `FOR SYSTEM_TIME AS OF STARTED IN (delta_num1, delta_num2)`, если хотя бы одна дельта из диапазона
  отсутствует в материализованном представлении;
* `FOR SYSTEM_TIME AS OF FINISHED IN (delta_num1, delta_num2)`, если хотя бы одна дельта из диапазона
  отсутствует в материализованном представлении.
  
### Поддерживаемые типы соединений (префиксы JOIN) {#join_prefixes}

Поддерживаются следующие типы соединений:
*   `[INNER]` — внутреннее соединение,
*   `NATURAL` — внутреннее соединение по всем столбцам с одинаковыми именами, ключи соединения 
    не указываются,
*   `LEFT [OUTER]` — левое внешнее соединение,
*   `RIGHT [OUTER]` — правое внешнее соединение,
*   `FULL [OUTER]` — полное внешнее соединение,
*   `CROSS` — декартово произведение таблиц или представлений, ключи соединения не указываются.

### Ключевое слово OFFSET {#offset}

Ключевое слово `OFFSET` позволяет пропустить первые несколько строк результата и выбрать только последующие строки. 
В качестве значения ключевого слова можно указать любое неотрицательное целое число, начиная с нуля, или 
переменную. Если для `OFFSET` указано значение 0, то пропускается 0 строк, что равносильно запросу 
без `OFFSET`. 

Запросы с `OFFSET` без ограничения количества строк не поддерживаются. То есть, если ключевое слово 
`OFFSET` указано в запросе, то перед ним должно быть ключевое слово `LIMIT <N>` или `FETCH NEXT <N> ROWS ONLY`. 
Обратного ограничения нет: ключевые слова `LIMIT <N>` и `FETCH NEXT <N> ROWS ONLY` можно использовать без `OFFSET`.

Рекомендуется сочетать `OFFSET` с ключевым словом `ORDER BY` для получения упорядоченного набора строк. Ключевое слово 
`ORDER BY` необязательно, однако без него запрос с `OFFSET` возвращает неупорядоченный и потому 
непредсказуемый набор строк. Примеры запросов см. [ниже](#offset_example).

Таким образом, для ключевого слова `OFFSET` поддерживается следующий синтаксис:
```sql
[ ORDER BY <column_name> ]
{ LIMIT <value_1> | FETCH NEXT <value_1> ROWS ONLY } 
OFFSET <value_2> [ ROW | ROWS ]
```

## Ограничения {#restrictions}

*   Запрос может обращаться либо к логической БД, либо к сервисной БД (см. [SELECT FROM INFORMATION_SCHEMA](../SELECT_FROM_INFORMATION_SCHEMA/SELECT_FROM_INFORMATION_SCHEMA.md)), 
    но не к обеим одновременно.
*   Если ключами соединения в запросе выступают поля типа Nullable, то строки, где хотя бы один из ключей 
    имеет значение NULL, не соединяются.
*   Для SELECT-подзапроса, указываемого в составе запроса [CREATE MATERIALIZED VIEW](../CREATE_MATERIALIZED_VIEW/CREATE_MATERIALIZED_VIEW.md), 
    не поддерживается ключевое слово `ORDER BY`. 

## Примеры {#examples}

### Звездочка и WHERE {#asterisk_and_where_example}

Запрос с неявным указанием столбцов и ключевым словом `WHERE`:
```sql
SELECT * FROM sales.sales
WHERE store_id = 1234
```

### DATASOURCE_TYPE {#datasource_type_example}

Запрос с явным указанием столбцов и выбором данных из определенной СУБД хранилища (ADQM):
```sql
SELECT sold.store_id, sold.product_amount
FROM sales.stores_by_sold_products AS sold
DATASOURCE_TYPE = 'adqm'
```

### GROUP BY, ORDER BY и LIMIT {#group_by_example}

Запрос с агрегацией, группировкой и сортировкой данных, а также выбором первых 20 строк:
```sql
SELECT s.store_id, SUM(s.product_units) AS product_amount
FROM sales.sales AS s
GROUP BY (s.store_id)
ORDER BY product_amount DESC
LIMIT 20
```

### OFFSET {#offset_example}

Запрос 20 строк, начиная с десятой:
```sql
SELECT * from sales.sales FETCH NEXT 20 ROWS ONLY OFFSET 9
```

### ORDER BY, LIMIT и OFFSET {#offset_and_order_by_example}

Запрос 20 строк, упорядоченных по значению `identification_number` и выбираемых начиная с десятой строки 
результата:
```sql
SELECT * from sales.sales ORDER BY identification_number LIMIT 20 OFFSET 9
```

Такое сочетание ключевых слов позволяет выбирать данные порциями с сохранением их порядка.

### FOR SYSTEM_TIME AS OF DELTA_NUM {#for_system_time_example}

Запрос записей, актуальных на момент закрытия дельты с номером 9, из материализованного представления:
```sql
SELECT * FROM sales.sales_and_stores FOR SYSTEM_TIME AS OF DELTA_NUM 9
```

### Соединение таблиц из разных логических БД {#join_example}

Запрос с соединением данных двух логических таблиц из двух различных логических БД:
```sql
SELECT
  st.identification_number,
  st.category,
  s.product_code
FROM sales.stores FOR SYSTEM_TIME AS OF LATEST_UNCOMMITTED_DELTA AS st
INNER JOIN sales2.sales FOR SYSTEM_TIME AS OF LATEST_UNCOMMITTED_DELTA AS s
  ON st.identification_number = s.store_id
```

### Соединение изменений из разных дельт {#deltas_example}

Запрос с соединением записей логической таблицы, добавленных и измененных в двух различных диапазонах 
дельт:
```sql
-- выбор логической базы данных sales в качестве базы данных по умолчанию
use sales

-- запрос данных из логической таблицы prices
SELECT
  p1.product_code,
  p1.price as feb_price,
  p2.price as march_price,
  (p2.price - p1.price) as diff
FROM
  (SELECT product_code,
  price from sales.prices
  FOR SYSTEM_TIME STARTED IN(3,6)) AS p1
FULL JOIN (select product_code,
  price from sales.prices
  FOR SYSTEM_TIME STARTED IN(7,10)) AS p2
  ON p1.product_code = p2.product_code
WHERE p1.product_code is NOT NULL
ORDER BY diff DESC
LIMIT 50
DATASOURCE_TYPE = 'adb'
```