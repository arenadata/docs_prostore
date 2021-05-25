---
layout: default
title: Поддержка SQL
nav_order: 2
parent: Справочная информация
has_children: false
has_toc: false
---

# Поддержка SQL

В [SELECT](../Запросы_SQLplus/SELECT/SELECT.md)-запросах к данным можно использовать функции, описанные 
в таблице ниже.

[СУБД](../../Введение/Поддерживаемые_СУБД_хранилища/Поддерживаемые_СУБД_хранилища.md) [хранилища](../../Обзор_понятий_компонентов_и_связей/Основные_понятия/Хранилище_данных/Хранилище_данных.md) 
имеют ограничения на использование некоторых функций в запросах, вызванные особенностями этих СУБД. 
Наиболее полный синтаксис запросов доступен в ADB.

| Функция | Доступна в ADB | Доступна в ADQM | Доступна в ADG
|:-|:-:|:-:|:-:
| ABS | + | + | +
| ACOS | + | + | −
| ASIN | + | + | −
| ATAN | + | + | −
| ATAN2 | + | − | −
| AVG | − | − | −
| BIT_AND | + | − | −
| BIT_OR | + | − | −
| CASE | + | + | +
| CAST | + | + | +
| CBRT | + | − | −
| CEIL | +<br>аргумент FLOAT | + | −
| CEILING | +<br>аргумент FLOAT | + | −
| CHAR | − | − | −
| COALESCE | + | + | +
| COS | + | + | −
| COUNT | + | + | +
| CROSS JOIN | + | − | +
| DEGREES | + | − | −
| EXCEPT | − | − | −
| EXP | + | + | −
| FLOOR | +<br>аргумент FLOAT | + | −
| FULL JOIN | + | − | −
| GREATEST | − | − | −
| HEX | − | − | −
| IFNULL | − | − | −
| INTERSECT | − | − | −
| JOIN для трех таблиц | + | − | +
| JOIN с подзапросом | + | − | +
| LEAST | − | − | −
| LENGTH | − | − | −
| LIKELY | − | − | −
| LN | + | + | −
| LOG | − | − | −
| LOWER | + | + | +
| MAX | + | + | +
| MIN | + | + | +
| MOD | + | − | −
| NULLIF | + | + | +
| OCTET_LENGTH | − | − | −
| PI | − | − | −
| POSITION | + | − | −
| POWER | + | + | −
| PRINTF | − | − | −
| QUOTE | − | − | −
| RADIANS | + | − | −
| RANDOM | − | − | −
| REPLACE | + | + | +
| RIGHT JOIN | + | + | −
| ROUND | +<br>аргумент FLOAT | + | −
| SIGN | +<br>аргумент FLOAT | − | −
| SIN | + | + | −
| SQRT | + | + | −
| SUBSTRING | + | + | −
| SUM | + | + | +
| TAN | + | + | −
| TRIM | + | + | +
| TRUNC | − | − | −
| TYPEOF | − | − | −
| UNION | − | − | −
| UPPER | + | + | +

## Примеры неподдерживаемых запросов

### AVG

```sql
SELECT AVG(product_units) FROM sales.sales
```

### CROSS JOIN

```sql
SELECT *
FROM sales.sales AS s
CROSS JOIN sales.stores AS st
ORDER BY s.store_id, st.category
LIMIT 5
DATASOURCE_TYPE = 'ADQM'
```

### FULL JOIN

```sql
SELECT *
FROM sales.sales AS s
FULL JOIN sales.stores AS st
ON s.store_id = st.identification_number
ORDER BY s.store_id
LIMIT 5
DATASOURCE_TYPE = 'ADG'
```

### JOIN для трех таблиц

```sql
SELECT *
FROM demo.territories AS t
LEFT JOIN demo.employee_territories AS et
ON t.territory_id = et.territory_id
LEFT JOIN demo.employees AS e
ON t.territory_id = e.territory_id
WHERE e.last_name is NOT NULL
ORDER BY t.territory_id
DATASOURCE_TYPE = 'ADQM'
```

### JOIN с подзапросом

```sql
SELECT *
FROM sales.sales AS s
INNER JOIN
(SELECT * FROM sales.stores) AS st
ON s.store_id = st.identification_number
ORDER BY s.store_id
DATASOURCE_TYPE = 'ADQM'
```

### RIGHT JOIN

```sql
SELECT *
FROM sales.sales AS s
RIGHT JOIN sales.stores AS st
ON s.store_id = st.identification_number
ORDER BY st.identification_number
LIMIT 5
DATASOURCE_TYPE = 'ADG'
```