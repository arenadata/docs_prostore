---
layout: default
title: Запрос данных
nav_order: 5
parent: Работа с системой
has_children: true
has_toc: false
---

# Запрос данных

Система позволяет запрашивать небольшие объемы актуальных и архивных данных, а также изменений, 
выполненных в рамках указанных дельт. Возможные способы выборки данных описаны в секции 
[FOR SYSTEM_TIME](../../reference/sql_plus_requests/SELECT/SELECT.md#sect_for_system_time) раздела [SELECT](../../reference/sql_plus_requests/SELECT/SELECT.md).

**Примечание:** под небольшим объемом данных подразумевается результат, содержащий десятки строк.
Для запроса большого объема данных следует использовать функцию [выгрузки данных](../data_download/data_download.md).

Чтобы запросить небольшой объем данных из [логических таблиц](../../overview/main_concepts/logical_table/logical_table.md), 
[логических представлений](../../overview/main_concepts/logical_view/logical_view.md) 
или [материализованного представления](../../overview/main_concepts/materialized_view/materialized_view.md), 
выполните запрос [SELECT](../../reference/sql_plus_requests/SELECT/SELECT.md). Запросы на чтение 
данных обрабатываются в порядке, описанном в разделе 
[Порядок обработки запросов на чтение данных](../../overview/interactions/llr_processing/llr_processing.md). 
При успешном выполнении запроса запрошенные данные возвращаются в ответе.

На рисунке ниже показан пример запроса из логической таблицы `sales`, возвращающего одну строку. 
Так как секция `DATASOURCE_TYPE` не указана, система автоматически направляет запрос в СУБД, оптимальную 
для его исполнения (см. [Маршрутизация запросов к данным](routing/routing.md)).

![](data_reading.png)
{: .figure-center}
*Запрос небольшого объема данных*
{: .figure-caption-center}

## Пример

```sql
-- выбор логической базы данных sales в качестве базы данных по умолчанию
USE sales

-- запрос данных из логической таблицы sales
SELECT s.store_id, SUM(s.product_units) AS product_amount
FROM sales AS s
GROUP BY (s.store_id)
ORDER BY product_amount DESC
LIMIT 20

-- запрос данных из логического представления stores_by_sold_products
SELECT sold.store_id, sold.product_amount
FROM stores_by_sold_products AS sold
```