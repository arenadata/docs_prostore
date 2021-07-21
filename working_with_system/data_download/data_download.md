---
layout: default
title: Выгрузка данных
nav_order: 4
parent: Работа с системой
has_children: false
---

# Выгрузка данных

Система позволяет выгружать актуальные и архивные данные, а также совокупность изменений, выполненных 
в рамках указанных дельт. Возможные способы выбора данных к выгрузке описаны в секции 
[FOR SYSTEM_TIME](../../reference/sql_plus_requests/SELECT/SELECT.md#sect_for_system_time) раздела [SELECT](../../reference/sql_plus_requests/SELECT/SELECT.md).

Для выгрузки данных нужен существующий топик Kafka. Если в брокере сообщений Kafka настроено 
автоматическое создание топиков, то дополнительные действия не требуются. Иначе необходимо создать топик, 
если он еще не создан. Подробнее о создании топиков см. в документации Kafka:
*   раздел [Quick Start](https://kafka.apache.org/documentation/#quickstart),
*   раздел [Adding and removing topics](https://kafka.apache.org/documentation/#basic_ops_add_topic).

**Примечание:** выгрузка данных возможна из [логических таблиц](../../overview/main_concepts/logical_table/logical_table.md) 
и [логических представлений](../../overview/main_concepts/logical_view/logical_view.md).
Выгрузка данных из [материализованных представлений](../../overview/main_concepts/materialized_view/materialized_view.md)
недоступна.

Чтобы выгрузить данные из логической таблицы или логического представления во внешнюю информационную систему:
1.  [Создайте](../../reference/sql_plus_requests/CREATE_DOWNLOAD_EXTERNAL_TABLE/CREATE_DOWNLOAD_EXTERNAL_TABLE.md) 
    [внешнюю таблицу](../../overview/main_concepts/external_table/external_table.md) 
    выгрузки, если она еще не создана.
2.  Выполните запрос [INSERT INTO download_external_table](../../reference/sql_plus_requests/INSERT_INTO_download_external_table/INSERT_INTO_download_external_table.md) 
    на выгрузку данных в топик. В запросе нужно указать внешнюю таблицу выгрузки, определяющую параметры 
    выгрузки.
3.  Выгрузите данные из топика во внешнюю информационную систему.

Созданные внешние таблицы выгрузки можно использовать повторно или удалить.

## Пример
```sql
-- выбор логической базы данных sales в качестве базы данных по умолчанию
USE sales

-- создание внешней таблицы выгрузки
CREATE DOWNLOAD EXTERNAL TABLE sales_ext_download (
  identification_number INT,
  transaction_date TIMESTAMP,
  product_code VARCHAR(256),
  product_units INT,
  store_id INT,
  description VARCHAR(256)
)
LOCATION  'kafka://zk1:2181,zk2:2181,zk3:2181/sales_out'
FORMAT 'AVRO'
CHUNK_SIZE 1000

-- запуск выгрузки данных из логической таблицы sales
INSERT INTO sales_ext_download SELECT * FROM sales WHERE product_units > 2
```