---
layout: default
title: Поддержка SQL
nav_order: 2
parent: Справочная информация
has_children: false
has_toc: false
---

# Поддержка SQL {#sql_support}
{: .no_toc }

<details markdown="block">
  <summary>
    Содержание раздела
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

В [SELECT](../sql_plus_requests/SELECT/SELECT.md)-запросах к данным можно использовать функции и операторы, описанные 
в таблице ниже.

[СУБД](../../introduction/supported_DBMS/supported_DBMS.md) [хранилища](../../overview/main_concepts/data_storage/data_storage.md) 
имеют ограничения на использование некоторых функций в запросах, вызванные особенностями этих СУБД. 
Наиболее полный синтаксис запросов доступен в ADB и ADP.


*Функции и операторы даты и времени*

<table>

<tr><td> Функция </td><td> Запрос </td><td> СУБД </td></tr>
<tr><td>

`CURRENT_DATE`
  
</td><td>

```sql
SELECT CAST(CURRENT_DATE AS DATE) FROM table1 datasource_type = 'ADB';
SELECT CAST(CURRENT_DATE AS DATE) FROM table1 datasource_type = 'ADP';
```

</td><td>ADB, ADP</td></tr>
<tr><td>

`CURRENT_DATE`
  
</td><td>

```sql
SELECT CURRENT_DATE FROM table1 datasource_type = 'ADB';
SELECT CURRENT_DATE FROM table1 datasource_type = 'ADP';
```

</td><td>ADB, ADP</td></tr>
<tr><td>
  
`CURRENT_TIME`
 
 </td><td>
   
```sql
SELECT CAST(CURRENT_TIME AS TIME) FROM table1 datasource_type = 'ADB';
SELECT CAST(CURRENT_TIME AS TIME) FROM table1 datasource_type = 'ADP';
```
 
</td><td>ADB, ADP</td></tr>
<tr><td>

`CURRENT_TIME`

</td><td>
  
```sql
SELECT CURRENT_TIME FROM table1 datasource_type = 'ADB';
SELECT CURRENT_TIME FROM table1 datasource_type = 'ADP';
```

</td><td>–</td></tr>
<tr><td>

`CURRENT_TIMESTAMP`
  
</td><td>
  
```sql
SELECT CAST(CURRENT_TIMESTAMP AS TIMESTAMP) FROM table1 datasource_type = 'ADB';
SELECT CAST(CURRENT_TIMESTAMP AS TIMESTAMP) FROM table1 datasource_type = 'ADP';
```

</td><td>ADB, ADP</td></tr>
<tr><td>

`CURRENT_TIMESTAMP`
  
 </td><td>
  
```sql
SELECT CURRENT_TIMESTAMP FROM table1 datasource_type = 'ADB';
SELECT CURRENT_TIMESTAMP FROM table1 datasource_type = 'ADP';
```

</td><td>–</td></tr>
<tr><td>
  
`EXTRACT(FROM DATE)`
  
</td><td>

```sql
SELECT CAST(EXTRACT(EPOCH FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(DOY FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(DOW FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(WEEK FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(CENTURY FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(QUARTER FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT EXTRACT(QUARTER FROM DATE '2001-02-16') FROM table1 datasource_type = 'ADQM';
SELECT CAST(EXTRACT(YEAR FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT EXTRACT(YEAR FROM DATE '2001-02-16') FROM table1 datasource_type = 'ADQM';
SELECT CAST(EXTRACT(MONTH FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT EXTRACT(MONTH FROM DATE '2001-02-16') FROM table1 datasource_type = 'ADQM';
SELECT CAST(EXTRACT(MONTH FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADP';
SELECT CAST(EXTRACT(DAY FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT EXTRACT(DAY FROM DATE '2001-02-16') FROM table1 datasource_type = 'ADQM';
SELECT CAST(EXTRACT(DAY FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADP';
```

</td><td>ADB, ADQM, ADP</td></tr>
<tr><td>  

`EXTRACT(FROM DATE)`

</td><td>

 ```sql
SELECT CAST(EXTRACT(DECADE FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(DECADE FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADP';
SELECT CAST(EXTRACT(ISOYEAR FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(ISOYEAR FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADP';
SELECT CAST(EXTRACT(ISODOW FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(ISODOW FROM DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADP';
```
  
</td><td>ADB, ADP</td></tr>
<tr><td>  

`EXTRACT(FROM TIME)`

</td><td>

```sql
SELECT CAST(EXTRACT(HOUR FROM TIME '20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(MINUTE FROM TIME '20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(SECOND FROM TIME '20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(MILLISECOND FROM TIME '20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(MICROSECOND FROM TIME '20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
```

</td><td>ADB, ADP</td></tr>
<tr><td>

| `EXTRACT(FROM TIMESTAMP)`

</td><td>

```sql
SELECT CAST(EXTRACT(DOW FROM TIMESTAMP '2001-02-16 00:00:00') AS INT) FROM table1 datasource_type = 'adb';
SELECT CAST(EXTRACT(WEEK FROM TIMESTAMP '2001-02-16 00:00:00') AS INT) FROM table1 datasource_type = 'adb';
SELECT CAST(EXTRACT(CENTURY FROM TIMESTAMP '2001-02-16 00:00:00') AS INT) FROM table1 datasource_type = 'adb';
SELECT CAST(EXTRACT(QUARTER FROM TIMESTAMP '2001-02-16 00:00:00') AS INT) FROM table1 datasource_type = 'ADB';
SELECT EXTRACT(QUARTER FROM TIMESTAMP '2001-02-16 20:38:40') FROM table1 datasource_type = 'ADQM';
SELECT CAST(EXTRACT(YEAR FROM TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT EXTRACT(YEAR FROM TIMESTAMP '2001-02-16 20:38:40') FROM table1 datasource_type = 'ADQM';
SELECT CAST(EXTRACT(MONTH FROM TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT EXTRACT(MONTH FROM TIMESTAMP '2001-02-16 20:38:40') FROM table1 datasource_type = 'ADQM';
SELECT CAST(EXTRACT(DAY FROM TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT EXTRACT(DAY FROM TIMESTAMP '2001-02-16 20:38:40') FROM table1 datasource_type = 'ADQM';
SELECT CAST(EXTRACT(HOUR FROM TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT EXTRACT(HOUR FROM TIMESTAMP '2001-02-16 20:38:40') FROM table1 datasource_type = 'ADQM';
SELECT CAST(EXTRACT(MINUTE FROM TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT EXTRACT(MINUTE FROM TIMESTAMP '2001-02-16 20:38:40') FROM table1 datasource_type = 'ADQM';
SELECT CAST(EXTRACT(SECOND FROM TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT EXTRACT(SECOND FROM TIMESTAMP '2001-02-16 20:38:40') FROM table1 datasource_type = 'ADQM';
SELECT CAST(EXTRACT(MILLISECOND FROM TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(EXTRACT(MICROSECOND FROM TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
```

</td><td>ADB, ADQM, ADP</td></tr>
<tr><td>

`LOCALTIME`

</td><td>

```sql
SELECT LOCALTIME FROM table1 datasource_type = 'ADB';
SELECT LOCALTIME FROM table1 datasource_type = 'ADP';
SELECT CAST(LOCALTIME AS TIME) FROM table1 datasource_type = 'ADB';
SELECT CAST(LOCALTIME AS TIME) FROM table1 datasource_type = 'ADP';
```

</td><td>ADB, ADP</td></tr>
<tr><td>

`LOCALTIME(precision)`

</td><td>  

```sql
SELECT LOCALTIME(3) FROM table1 datasource_type = 'ADB';
SELECT LOCALTIME(3) FROM table1 datasource_type = 'ADP';
SELECT CAST(LOCALTIME(3) AS TIME) FROM table1 datasource_type = 'ADB';
SELECT CAST(LOCALTIME(3) AS TIME) FROM table1 datasource_type = 'ADP';
```

</td><td>ADB, ADP</td></tr>
<tr><td>

`LOCALTIMESTAMP`

</td><td>

```sql
SELECT LOCALTIMESTAMP FROM table1 datasource_type = 'ADB';
SELECT LOCALTIMESTAMP FROM table1 datasource_type = 'ADP';
SELECT CAST(LOCALTIMESTAMP AS TIMESTAMP) FROM table1 datasource_type = 'ADB';
SELECT CAST(LOCALTIMESTAMP AS TIMESTAMP) FROM table1 datasource_type = 'ADP';
```

</td><td>ADB, ADP</td></tr>
<tr><td>

`LOCALTIMESTAMP(precision)`

</td><td>

```sql
SELECT LOCALTIMESTAMP(3) FROM table1 datasource_type = 'ADB';
SELECT LOCALTIMESTAMP(3) FROM table1 datasource_type = 'ADP';
SELECT CAST(LOCALTIMESTAMP(3) AS TIMESTAMP) FROM table1 datasource_type = 'ADB';
SELECT CAST(LOCALTIMESTAMP(3) AS TIMESTAMP) FROM table1 datasource_type = 'ADP';
```

</td><td>ADB, ADP</td></tr>
<tr><td>
  
`MONTH()`

</td><td>

```sql
SELECT CAST(MONTH(DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(MONTH(DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADP';
SELECT CAST(MONTH(TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(MONTH(TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADP';
```

</td><td>ADB, ADP</td></tr>
<tr><td>

`QUARTER()`

</td><td>
  
```sql
SELECT CAST(QUARTER(DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(QUARTER(DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADP';
SELECT CAST(QUARTER(TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(QUARTER(TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADP';
```

</td><td>ADB, ADP</td></tr>
<tr><td>

`WEEK()`

</td><td>

```sql
SELECT CAST(WEEK(DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(WEEK(DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADP';
SELECT CAST(WEEK(TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(WEEK(TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADP';
```

</td><td>ADB, ADP</td></tr> 
<tr><td>

`YEAR()`

</td><td>

```sql
SELECT CAST(YEAR(DATE '2001-02-16') AS INT) FROM table1 datasource_type = 'ADB';
SELECT CAST(YEAR(TIMESTAMP '2001-02-16 20:38:40') AS INT) FROM table1 datasource_type = 'ADB';
```

</td><td>ADB, ADQM, ADP</td></tr> 

</table>

| Функция | Доступна в ADB | Доступна в ADQM | Доступна в ADG | Доступна в ADP
|:-|:-:|:-:|:-:|:-:
| ABS | + | + | + | +
| ACOS | + | + | − | +
| ASIN | + | + | − | +
| ATAN | + | + | − | +
| ATAN2 | + | − | − | +
| AVG | − | − | − | −
| BIT_AND | + | − | − | +
| BIT_OR | + | − | − | +
| CASE | + | + | + | +
| CAST | + | + | + | +
| CBRT | + | − | − | +
| CEIL | +<br>для аргумента типа FLOAT | + | − | +<br>для аргумента типа FLOAT
| CEILING | +<br>для аргумента типа FLOAT | + | − | +<br>для аргумента типа FLOAT
| CHAR | − | − | − | −
| COALESCE | + | + | + | +
| COS | + | + | − | +
| COUNT | + | + | + | +
| CROSS JOIN | + | − | + | +
| DEGREES | + | − | − | +
| EXCEPT | − | − | − | −
| EXP | + | + | − | +
| FLOOR | +<br>для аргумента типа FLOAT | + | − | +<br>для аргумента типа FLOAT
| FULL JOIN | + | +<br>при соединении по ключам шардирования | − | +
| GREATEST | − | − | − | −
| HEX | − | − | − | −
| IFNULL | − | − | − | −
| INTERSECT | − | − | − | −
| JOIN для трех таблиц | + | − | + | +
| JOIN с подзапросом | + | − | + | +
| LEAST | − | − | − | −
| LENGTH | − | − | − | −
| LIKE | + | + | + | +
| LN | + | + | − | +
| LOG | − | − | − | −
| LOWER | + | + | + | +
| MAX | + | + | + | +
| MIN | + | + | + | +
| MOD | + | − | − | +
| NULLIF | + | + | + | +
| OCTET_LENGTH | − | − | − | −
| PI | − | − | − | −
| POSITION | + | − | − | +
| POWER | + | + | − | +
| PRINTF | − | − | − | −
| QUOTE | − | − | − | −
| RADIANS | + | − | − | +
| RANDOM | − | − | − | −
| REPLACE | + | + | + | +
| RIGHT JOIN | + | + | − | +
| ROUND | +<br>для аргумента типа FLOAT | + | + | +<br>для аргумента типа FLOAT
| SIGN | +<br>для аргумента типа FLOAT | − | − | +<br>для аргумента типа FLOAT
| SIN | + | + | − | +
| SQRT | + | + | − | +
| SUBSTRING | + | + | − | +
| SUM | + | + | + | +
| TAN | + | + | − | +
| TRIM | + | + | + | +
| TRUNC | − | − | − | −
| TYPEOF | − | − | − | −
| UNION | − | − | − | −
| UPPER | + | + | + | +

