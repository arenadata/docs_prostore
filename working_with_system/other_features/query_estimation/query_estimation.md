---
layout: default
title: Получение информации о SELECT-запросе
nav_order: 2
parent: Другие функции
grand_parent: Работа с системой
has_children: false
has_toc: false
---

# Получение информации о SELECT-запросе {#query_estimation}

По [SELECT](../../../reference/sql_plus_requests/SELECT/SELECT.md)-запросу можно получить информацию 
о его выполнении без фактического выполнения в [СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) 
[хранилища](../../../overview/main_concepts/data_storage/data_storage.md). Доступна следующая информация:
* план выполнения запроса (только для запросов к ADB и ADP),
* модифицированный запрос, сформированный системой на основе исходного запроса с учетом специфики СУБД хранилища.

Чтобы получить информацию о запросе, выполните запрос с ключевым словом `ESTIMATE_ONLY` (см. пример [ниже](#examples)).

Подробнее о формате и содержимом ответа см. в секции 
[Ключевое слово ESTIMATE_ONLY](../../../reference/sql_plus_requests/SELECT/SELECT.md#estimate)
раздела [SELECT](../../../reference/sql_plus_requests/SELECT/SELECT.md), о ключевых словах SELECT-запроса 
см. в секции [Поддерживаемые ключевые слова](../../../reference/sql_plus_requests/SELECT/SELECT.md#key_words) 
того же раздела.

## Пример {#examples}

Запрос на получение информации о выполнении запроса:
```sql
SELECT s.store_id, SUM(s.product_units) AS product_amount
FROM sales.sales AS s
GROUP BY (s.store_id)
ORDER BY product_amount DESC
ESTIMATE_ONLY
```