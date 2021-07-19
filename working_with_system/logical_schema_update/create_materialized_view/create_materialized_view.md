---
layout: default
title: Создание материализованного представления
nav_order: 8
parent: Управление схемой данных
grand_parent: Работа с системой
has_children: false
---

# Создание материализованного представления

Чтобы создать [материализованное представление](../../../overview/main_concepts/materialized_view/materialized_view.md) 
в [логической базе данных](../../../overview/main_concepts/logical_db/logical_db.md), 
выполните запрос [CREATE MATERIALIZED VIEW](../../../reference/sql_plus_requests/CREATE_MATERIALIZED_VIEW/CREATE_MATERIALIZED_VIEW.md) 
(см. пример [ниже](#пример)). При успешном выполнении запроса материализованное представление появляется 
в [логической схеме данных](../../../overview/main_concepts/logical_schema/logical_schema.md).

Наличие материализованного представления можно проверить, как описано в разделе [Проверка наличия материализованного представления](../entity_presence_check/entity_presence_check.md#проверка-наличия-материализованного-представления).

## Пример

```sql
-- выбор базы данных sales по умолчанию
USE sales

-- создание материализованного представления sales_and_stores
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
