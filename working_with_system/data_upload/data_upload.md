---
layout: default
title: Загрузка данных
nav_order: 3
parent: Работа с системой
has_children: true
has_toc: false
---

# Загрузка данных {#data_upload}

Для загрузки данных нужен существующий топик Kafka. Если в брокере сообщений Kafka настроено 
автоматическое создание топиков, то дополнительные действия не требуются. Иначе необходимо создать топик, 
если он еще не создан. Подробнее о создании топиков см. в документации Kafka:
*   раздел [Quick Start](https://kafka.apache.org/documentation/#quickstart),
*   раздел [Adding and removing topics](https://kafka.apache.org/documentation/#basic_ops_add_topic).

**Примечание:** загрузка данных возможна только в [логическую таблицу](../../overview/main_concepts/logical_table/logical_table.md). 
Загрузка данных в [логические](../../overview/main_concepts/logical_view/logical_view.md) 
и [материализованные представления](../../overview/main_concepts/materialized_view/materialized_view.md)
недоступна.

Чтобы загрузить данные из внешней информационной системы в логическую таблицу:
1.  Загрузите данные из внешней информационной системы в топик Kafka.  
    Данные должны иметь формат, описанный в разделе [Формат загрузки данных](../../reference/upload_format/upload_format.md).
2.  [Создайте](../../reference/sql_plus_requests/CREATE_TABLE/CREATE_TABLE.md) 
    логическую таблицу, если она еще не создана.
3.  [Создайте](../../reference/sql_plus_requests/CREATE_UPLOAD_EXTERNAL_TABLE/CREATE_UPLOAD_EXTERNAL_TABLE.md) 
    [внешнюю таблицу](../../overview/main_concepts/external_table/external_table.md) 
    загрузки, если она еще не создана.
4.  Выполните запрос [BEGIN DELTA](../../reference/sql_plus_requests/BEGIN_DELTA/BEGIN_DELTA.md) 
    на открытие [дельты](../../overview/main_concepts/delta/delta.md), 
    если она еще не открыта.
5.  Выполните запрос [INSERT INTO logical_table](../../reference/sql_plus_requests/INSERT_INTO_logical_table/INSERT_INTO_logical_table.md) 
    на загрузку данных из топика в логическую таблицу. В запросе нужно указать внешнюю таблицу загрузки, 
    определяющую параметры загрузки.  
    После успешного окончания загрузки данных система вернет ответ с пустым объектом ResultSet.
6.  Если необходимо, загрузите другие данные, например в другие логические таблицы.  
    В рамках одной открытой дельты можно выполнять произвольное количество запросов 
    [INSERT INTO logical_table](../../reference/sql_plus_requests/INSERT_INTO_logical_table/INSERT_INTO_logical_table.md), 
    при этом не допускается загрузка различных состояний одного и того же объекта.
7.  Выполните запрос [COMMIT DELTA](../../reference/sql_plus_requests/COMMIT_DELTA/COMMIT_DELTA.md) 
    для сохранения изменений и закрытия дельты.
    
При успешном выполнении последовательности действий загруженные данные сохраняются в качестве актуальных, 
а предыдущая версия данных, если такая была, становится архивной. Подробнее о версионировании см. 
в разделе [Версионирование данных](data_versioning/data_versioning.md).

Пока дельта не закрыта, все изменения данных, выполненные в рамках нее, можно отменить 
(см. [ROLLBACK DELTA](../../reference/sql_plus_requests/ROLLBACK_DELTA/ROLLBACK_DELTA.md)). 
Созданные внешние таблицы загрузки можно использовать повторно или удалить.

## Пример {#example}
```sql
-- выбор логической базы данных sales в качестве базы данных по умолчанию
USE sales

-- создание логической таблицы sales
CREATE TABLE sales (
  identification_number INT NOT NULL,
  transaction_date TIMESTAMP NOT NULL,
  product_code VARCHAR(256) NOT NULL,
  product_units INT NOT NULL,
  store_id INT NOT NULL,
  description VARCHAR(256),
  PRIMARY KEY (identification_number)
)
DISTRIBUTED BY (identification_number)

-- создание внешней таблицы загрузки
CREATE UPLOAD EXTERNAL TABLE sales_ext_upload (
  identification_number INT,
  transaction_date TIMESTAMP,
  product_code VARCHAR(256),
  product_units INT,
  store_id INT,
  description VARCHAR(256)
)
LOCATION  'kafka://zk1:2181,zk2:2181,zk3:2181/sales'
FORMAT 'AVRO'
MESSAGE_LIMIT 1000

-- открытие новой (горячей) дельты
BEGIN DELTA

-- запуск загрузки данных в логическую таблицу sales
INSERT INTO sales SELECT * FROM sales.sales_ext_upload

-- закрытие дельты (фиксация изменений)
COMMIT DELTA
```