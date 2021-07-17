---
layout: default
title: Изменение логического представления
nav_order: 6
parent: Управление схемой данных
grand_parent: Работа с системой
has_children: false
---

# Изменение логического представления

Чтобы изменить [логическое представление](../../../overview/main_concepts/logical_view/logical_view.md) 
в [логической БД](../../../overview/main_concepts/logical_db/logical_db.md), 
выполните запрос [ALTER VIEW](../../../reference/sql_plus_requests/ALTER_VIEW/ALTER_VIEW.md) 
или `CREATE OR REPLACE VIEW` (см. [CREATE VIEW](../../../reference/sql_plus_requests/CREATE_VIEW/CREATE_VIEW.md)).

При успешном выполнении запроса логическое представление изменит свой вид.

## Пример

```sql
-- выбор sales как логической базы данных по умолчанию
USE sales

-- создание логического представления
CREATE VIEW stores_by_sold_products AS
  SELECT store_id, SUM(product_units) AS product_amount
  FROM sales
  GROUP BY store_id
  ORDER BY product_amount DESC
  LIMIT 10

-- изменение логического представления
ALTER VIEW stores_by_sold_products AS
  SELECT store_id, SUM(product_units) AS product_amount
  FROM sales
  GROUP BY store_id
  ORDER BY product_amount ASC
  LIMIT 20

-- пересоздание логического представления
CREATE OR REPLACE VIEW stores_by_sold_products AS
  SELECT store_id, SUM(product_units) AS product_amount
  FROM sales
  GROUP BY store_id
  ORDER BY product_amount DESC
  LIMIT 30
```