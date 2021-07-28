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
(см. пример [ниже](#examples)). При успешном выполнении запроса логическая таблица появляется в 
[логической схеме данных](../../../overview/main_concepts/logical_schema/logical_schema.md).

Наличие логической таблицы можно проверить, как описано в разделе [Проверка наличия логической таблицы](../entity_presence_check/entity_presence_check.md#table_check).

## Пример {#examples}

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