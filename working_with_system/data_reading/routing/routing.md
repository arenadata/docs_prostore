---
layout: default
title: Маршрутизация запросов к данным
nav_order: 1
parent: Запрос данных
grand_parent: Работа с системой
has_children: false
has_toc: false
---

# Маршрутизация запросов к данным {#routing}
{: .no_toc }

<details markdown="block">
  <summary>
    Содержание раздела
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

[Запросы к данным](../../data_reading/data_reading.md) маршрутизируются следующим образом:
1.  Если в запросе указано ключевое слово [DATASOURCE_TYPE](../../../reference/sql_plus_requests/SELECT/SELECT.md#param_datasource_type) 
    с [СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) 
    [хранилища](../../../overview/main_concepts/data_storage/data_storage.md)
    для исполнения запроса, запрос направляется в указанную СУБД.
2.  Иначе:
    1. Определяются те СУБД хранилища, в которых можно выполнить запрос, — выбираются СУБД, содержащие 
       данные всех запрашиваемых логических сущностей.
    2. Определяется категория запроса.      
    3. Запрос направляется в ту из выбранных СУБД, которая имеет в [конфигурации](../../../maintenance/configuration/configuration.md) 
       самый высокий приоритет для запросов этой категории (см. [ниже](#tab_default_routing)).

Запросы к данным [материализованных представлений](../../../overview/main_concepts/materialized_view/materialized_view.md) 
после перечисленных этапов маршрутизации проходят дополнительные этапы 
(см. [Маршрутизация запросов к данным материализованных представлений](#mat_view_routing)). 

Порядок выбора СУБД для исполнения каждой категории запросов настраивается в конфигурации системы.
По умолчанию действует порядок, описанный в таблице ниже. Такой порядок выбора СУБД эффективно 
использует возможности каждой из СУБД хранилища, однако при необходимости его можно изменить.
<a id="tab_default_routing"></a>

| Категория запроса | Порядок выбора СУБД для исполнения запроса 
|:-|:-
| 1. Реляционный запрос (запрос с ключевым словом JOIN и (или) подзапросами) | 1. ADB<br>2. ADP<br>3. ADQM<br>4. ADG
| 2. Запрос агрегации и группировки | 1. ADQM<br>2. ADB<br>3. ADP<br>4. ADG
| 3. Запрос точечного чтения по ключу | 1. ADG<br>2. ADB<br>3. ADP<br>4. ADQM
| 4. Другой запрос (не соответствующий ни одной из предыдущих категорий) | 1. ADB<br>2. ADP<br>3. ADQM<br>4. ADG

Категория запроса определяется в указанном порядке: запрос проверяется на наличие признаков первой категории, 
и затем, если запрос не соответствует первой категории, он проверяется на наличие признаков второй категории и т.д. 
Например, запрос с ключевым словом JOIN соответствует первой категории независимо от наличия признаков других категорий — 
агрегации, группировки и чтения по ключу. Примеры запросов каждой из категорий см. [ниже](#requests_by_categories).

Наиболее полный синтаксис запросов доступен в ADB и ADP. ADG и ADQM имеют ограничения 
на выполнение запросов, вызванные особенностями этих СУБД (см. [Поддержка SQL](../../../reference/sql_support/sql_support.md)).
{: .note-wrapper}

## Примеры запросов по категориям {#requests_by_categories}

### Реляционные запросы {#relation}

Реляционный запрос:
```sql
SELECT * FROM sales.sales AS s
JOIN sales.stores AS st
ON s.store_id = st.identification_number
```

Реляционный запрос, который включает агрегацию, группировку и чтение по ключу (`st.identification_number`):
```sql
SELECT
st.identification_number,
st.category,
SUM(s.product_units) AS product_amount
FROM sales.stores AS st
JOIN sales.sales AS s
ON st.identification_number = s.store_id
WHERE st.identification_number <> 10004
GROUP BY st.identification_number, st.category
ORDER BY product_amount DESC
```

### Запросы агрегации и группировки {#aggregation}

Запрос агрегации и группировки:
```sql
SELECT s.product_code, SUM(s.product_units) AS product_amount
FROM sales.sales AS s
GROUP BY s.product_code
ORDER BY product_amount ASC
```

Запрос агрегации и группировки, который включает чтение по ключу (`s.identification_number`):
```sql
SELECT s.product_code, SUM(s.product_units) AS product_amount
FROM sales.sales AS s
WHERE s.identification_number > 20000
GROUP BY s.product_code
```

### Запрос чтения по ключу {#read_by_key}

```sql
SELECT * FROM sales.sales as s
WHERE s.identification_number BETWEEN 1001 AND 2000
```

### Запрос неопределенной категории {#undefined}

```sql
SELECT *
FROM sales.sales AS s
WHERE s.product_units > 2  
```

## Маршрутизация запросов к данным материализованных представлений {#mat_view_routing}

Запросы к данным [материализованных представлений](../../../overview/main_concepts/materialized_view/materialized_view.md) 
проходят все этапы маршрутизации, описанные [выше](#top), и затем — дополнительные этапы:
1. Если для материализованного представления **не** указано ключевое слово 
   [FOR SYSTEM_TIME](../../../reference/sql_plus_requests/SELECT/SELECT.md#for_system_time), 
   запрос направляется в СУБД, где размещены данные этого представления. Из представления выбираются данные, 
   актуальные на момент его последней синхронизации.
2. Иначе, если ключевое слово `FOR SYSTEM_TIME` указано, система проверяет, присутствуют ли запрашиваемые данные 
   в материализованном представлении.
   * Если данные присутствуют в представлении, запрос направляется в СУБД, где размещены данные этого представления.
   * Иначе запрос направляется к исходным таблицам СУБД-источника, на которых построено представление.
    
В запросах к материализованным представлениям доступны не все сочетания с ключевым словом `FOR SYSTEM_TIME`. 
Подробнее см. в секции 
[Ключевое слово FOR SYSTEM_TIME](../../../reference/sql_plus_requests/SELECT/SELECT.md#for_system_time_matview) 
раздела [SELECT](../../../reference/sql_plus_requests/SELECT/SELECT.md).
{: .note-wrapper}