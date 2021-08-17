---
layout: default
title: Создание логической таблицы
nav_order: 3
parent: Управление схемой данных
grand_parent: Работа с системой
has_children: false
---

# Создание логической таблицы {#create_table}

Чтобы создать [логическую таблицу](../../../overview/main_concepts/logical_table/logical_table.md) 
в [логической базе данных](../../../overview/main_concepts/logical_db/logical_db.md), 
выполните запрос [CREATE TABLE](../../../reference/sql_plus_requests/CREATE_TABLE/CREATE_TABLE.md) 
(см. примеры [ниже](#examples)):
* без ключевого слова `LOGICAL_ONLY` — чтобы создать логическую таблицу 
  в [логической схеме данных](../../../overview/main_concepts/logical_schema/logical_schema.md) и 
  связанные [физические таблицы](../../../overview/main_concepts/physical_table/physical_table.md) — 
  в [хранилище данных](../../../overview/main_concepts/data_storage/data_storage.md). 
  Созданная таким образом логическая таблица готова для загрузки, выгрузки и чтения данных;
* с ключевым словом `LOGICAL_ONLY` — чтобы создать логическую таблицу только на логическом уровне 
  (в логической схеме данных). Создание сущностей только на логическом уровне может быть полезно, например, 
  при изменении физической и логических схем данных.

Если данные логической таблицы нужно размещать только в некоторых [СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) 
хранилища, добавьте в запрос ключевое слово `DATASOURCE_TYPE` с псевдонимами требуемых СУБД 
(см. пример [ниже](#adqm_adg_example)). По умолчанию, если ключевое слово не указано в запросе, 
данные таблицы размещаются во всех доступных СУБД хранилища.
  
Наличие логической таблицы можно проверить, как описано в разделе 
[Проверка наличия логической таблицы](../entity_presence_check/entity_presence_check.md#table_check). Наличие 
физических таблиц, связанных с логической, можно проверить, как описано в разделе
[Проверка месторасположения логической сущности](../../../working_with_system/other_features/datasource_check/datasource_check.md).

## Примеры {#examples}

### Создание логической таблицы {#non-logical_example}

```sql
-- выбор базы данных sales по умолчанию
USE sales

-- создание таблицы sales
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
```

### Создание логической таблицы только на логическом уровне {#logical_example}

```sql
CREATE TABLE sales.sales1 (
  identification_number INT NOT NULL,
  transaction_date TIMESTAMP NOT NULL,
  product_code VARCHAR(256) NOT NULL,
  product_units INT NOT NULL,
  store_id INT NOT NULL,
  description VARCHAR(256),
  PRIMARY KEY (identification_number)
)
DISTRIBUTED BY (identification_number)
LOGICAL_ONLY
```

### Создание логической таблицы с размещением данных в ADQM и ADG {#adqm_adg_example}

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