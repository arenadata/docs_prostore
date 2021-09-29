---
layout: default
title: Приступая к работе
nav_order: 0.5
has_children: false
has_toc: false
---

# Приступая к работе
{: .no_toc }

<details markdown="block">
  <summary>
    Содержание раздела
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Предустановленные программные средства:
*   OC Centos 7;
*   yum-utils;
*   curl;
*   git;
*   wget;
*   OpenJDK 8;
*   Apache Maven 3.6.3;
*   СУБД PostgreSQL 13;
*   Apache Zookeeper;
*   Apache Kafka (например, в каталоге /opt/kafka);
*   Apache Avro (например, в каталоге /opt/avro);
*   SQL-клиент, например DBeaver;
*   Браузер kafka-топиков, например Offset Explorer 2.1.

## 1. Сборка Prostore:

```plaintext
# клонирование репозитория Prostore
git clone https://github.com/arenadata/prostore
# Запуск сборки Prostore средствами Apache Maven
cd ~/prostore
mvn clean
mvn install -DskipTests=true
# создание символической ссылки на файл конфигурации
sudo ln -s ~/prostore/dtm-query-execution-core/config/application.yml ~/prostore/dtm-query-execution-core/target/application.yml
# приведение конфигурационного файла к виду, показанному в приложении A
sudo nano ~/prostore/dtm-query-execution-core/config/application.yml
```
Далее конфигурационный файл `application.yml` обозначается термином "конфигурация Prostore".

## 2. Настройка СУБД Postgres:

```plaintext
# создание в СУБД Postgres SUPERUSER-пользователя c именем и паролем,
# указаннымыми в конфигурации Prostore
# (значения параметров (adp: datasource: user: ${ADP_USERNAME:dtm}) и (adp: datasource: password: ${ADP_PASS:dtm}) соответственно)
cd /
sudo -u postgres psql -c 'CREATE ROLE dtm WITH LOGIN SUPERUSER'
sudo -u postgres psql -c "ALTER ROLE dtm WITH PASSWORD 'dtm'"
# создание базы данных с именем test, указанным в конфигурации Prostore (env: name: ${DTM_NAME:test})
sudo -u postgres psql -c 'CREATE DATABASE test'
# перезапуск сервиса Postgresql:
sudo systemctl reload postgresql-13
```
## 3. Сборка и установка коннектора Kafka-Postgres:

```plaintext
# клонирование репозитория kafka-postgres-connector
git clone https://github.com/arenadata/kafka-postgres-connector
# запуск сборки коннектора kafka-postgres средствами Apache Maven
cd ~/kafka-postgres-connector
mvn clean
mvn install -DskipTests=true
# приводение конфигурационных файлов kafka-postgres-writer и kafka-postgres-reader к виду,
# показанному в приложениях B и C, чтобы значения параметров совпадали со значениями соответствующих параметров конфигурации Prostore
# datasource: postgres: database: ${POSTGRES_DB_NAME:} ~ env: name: ${DTM_NAME:},
# datasource: postgres: user: ${POSTGRES_USERNAME:}    ~ adp: datasource: user: ${ADP_USERNAME:},
# datasource: postgres: password: ${POSTGRES_PASS:}    ~ adp: datasource: password: ${ADP_PASS:},
# datasource: postgres: hosts: ${POSTGRES_HOSTS:}      ~ adp: datasource: host: ${ADP_HOST:}, adp: datasource: port: ${ADP_PORT:}
sudo nano ~/kafka-postgres-connector/kafka-postgres-writer/src/main/resources/application-default.yml
sudo nano ~/kafka-postgres-connector/kafka-postgres-reader/src/main/resources/application-default.yml
# создание символических ссылок на файлы конфигурации
sudo ln -s ~/kafka-postgres-connector/kafka-postrges-writer/src/main/resources/application-default.yml ~/kafka-postgres-connector/kafka-postrges-writer/target/application-default.yml
sudo ln -s ~/kafka-postgres-connector/kafka-postrges-reader/src/main/resources/application-default.yml ~/kafka-postgres-connector/kafka-postrges-reader/target/application-default.yml
```
## 4. Запуск сервисов Apache Zookeeper и Apache Kafka:

```plaintext
# запуск одного экземпляра сервера ZooKeeper, если он еще не запущен
sudo systemctl start zookeeper
# запуск сервера Kafka и проверка его состояния
sudo systemctl start kafka
sudo systemctl status kafka
```
## 5. Запуск коннектора Kafka-Postgres:

```plaintext
# запуск kafka-postgres-writer в отдельном окне терминала 
cd ~/kafka-postgres-connector/kafka-postgres-writer/target
java -Dspring.profiles.active=default -jar kafka-postgres-writer-<version>.jar
# запуск kafka-postgres-reader в отдельном окне терминала
cd ~/kafka-postgres-connector/kafka-postgres-reader/target
java -Dspring.profiles.active=default -jar kafka-postgres-reader-<version>.jar
```
## 6. Запуск службы dtm-status-monitor:

```plaintext
# создание символической ссылки на файл конфигурации dtm-status-monitor
sudo ln -s ~/prostore/dtm-status-monitor/src/main/resources/application.yml ~/prostore/dtm-status-monitor/target/application.yml
# запуск dtm-status-monitor в отдельном окне терминала с указанием порта, заданного в конфигурации Prostore (core:kafka:statusMonitor)
cd ~/prostore/dtm-status-monitor/target
java -Dspring.profiles.active=default -Dserver.port=9095 -jar dtm-status-monitor-<version>.jar
```
## 7. Запуск Prostore:

```plaintext
# запуск файла dtm-query-execution-core-<version>.jar (например, dtm-query-execution-core-5.1.0.jar):
cd ~/prostore/dtm-query-execution-core/target
java -jar dtm-query-execution-core-<version>.jar
Подключение к Prostore с помощью SQL-клиента (см раздел [Подключение с помощью SQL-клиента](../working_with_system/connection/connection_via_sql_client/connection_via_sql_client.md)):
```

Рис.1. Настройки JDBC-подключения SQL-клиента DBeaver к Prostore. 
 

# Демонстрационный сценарий
## 1. Создание необходимых логических сущностей:

```sql
-- создание логической базы данных
CREATE DATABASE sales;

-- выбор логической БД по умолчанию
USE sales;

-- создание логической таблицы в БД sales
CREATE TABLE sales (
  identification_number INT NOT NULL,
  transaction_date TIMESTAMP NOT NULL,
  product_code VARCHAR(256) NOT NULL,
  product_units INT NOT NULL,
  store_id INT NOT NULL,
  description VARCHAR(256),
  PRIMARY KEY (identification_number)
)
DISTRIBUTED BY (identification_number);

-- создание внешней таблицы загрузки
CREATE UPLOAD EXTERNAL TABLE sales_ext_upload (
  identification_number INT,
  transaction_date TIMESTAMP,
  product_code VARCHAR(256),
  product_units INT,
  store_id INT,
  description VARCHAR(256)
)
LOCATION  'kafka://localhost:2181/salesTopic'
FORMAT 'AVRO'
MESSAGE_LIMIT 1000;

-- создание логического представления stores_by_sold_products
CREATE VIEW stores_by_sold_products AS
  SELECT store_id, SUM(product_units) AS product_amount
  FROM sales.sales
  GROUP BY store_id
  ORDER BY product_amount DESC
  LIMIT 30;
```
## 2. Создание топика Kafka для последующей загрузки данных:
Создание Kafka-топика salesTopic с помощью программы Offset Explorer (Kafka tool) или в терминале

```plaintext
cd /opt/kafka/bin
bash kafka-topics.sh --create --replication-factor 1 --partitions 1 --topic salesTopic --zookeeper localhost:2181
```
Рис.2. Создание Kafka-топика с помощью программы Offset Explorer 2.1.
Создание бинарного avro-файла kafka_upload_sales.avro (приложение F) из avro-схемы (приложение D) и данных (приложение E).

## 3. Задание конфигурации kafka-топика, которая не использует поле ключа:


Рис.3. Конфигурация kafka-топика, которая не использует поле ключа.
## 4. Загрузка avro-файла kafka_upload_sales.avro в поле “значение” Kafka-топика salesTopic с помощью программы Offset Explorer 2.1:


Рис.4. Загрузка avro-файла в Kafka-топик
 

## 5. Загрузка данных:

```sql
-- открытие новой (горячей) дельты
BEGIN DELTA;
-- запуск загрузки данных в логическую таблицу sales
INSERT INTO sales SELECT * FROM sales.sales_ext_upload;
-- закрытие дельты (фиксация изменений)
COMMIT DELTA;
```
## 6. Вставка данных:

```sql
-- открытие новой (горячей) дельты
BEGIN DELTA;
-- запуск вставки данных в логическую таблицу sales
UPSERT INTO sales.sales
(identification_number, transaction_date, product_code, product_units, store_id, description)
VALUES
(2000111, '2020-05-01 13:14:16', 'ABC202010', 7, 1000000123, 'Покупка без акций'),
(2000112, '2020-05-02 16:13:17', 'ABC202011', 11, 1000000456, 'Покупка без акций'),
(2000113, '2020-05-03 21:15:17', 'ABC202012', 5, 1000000789, 'Покупка без акций'),
(2000114, '2020-05-04 23:03:13', 'ABC202013', 7, 1000000123, 'Покупка без акций'),
(2000115, '2020-05-05 14:10:21', 'ABC202014', 21, 1000000623, 'Покупка без акций'),
(2000116, '2020-06-12 08:43:56', 'ABC202015', 32, 1000000987, 'Покупка без акций');
-- закрытие дельты (фиксация изменений)
COMMIT DELTA;
```
## 7. Выборка данных:

```sql
-- запрос с неявным указанием столбцов и ключевым словом WHERE:
SELECT * FROM sales.sales
WHERE store_id = 1000000123;

-- запрос с агрегацией, группировкой и сортировкой данных, а также выбором первых 5 строк:
SELECT s.store_id, SUM(s.product_units) AS product_amount
FROM sales.sales AS s
GROUP BY (s.store_id)
ORDER BY product_amount DESC
LIMIT 5;

-- запрос к логическому представлению stores_by_sold_products
SELECT * from stores_by_sold_products;
```
## 8. Подготовка данных к выгрузке и выгрузка в kafka-топик salesTopicOut:

```sql
-- создание внешней таблицы выгрузки
CREATE DOWNLOAD EXTERNAL TABLE sales.sales_ext_download (
  identification_number INT,
  transaction_date TIMESTAMP,
  product_code VARCHAR(256),
  product_units INT,
  store_id INT,
  description VARCHAR(256)
)
LOCATION  'kafka://localhost:2181/salesTopicOut'
FORMAT 'AVRO'
CHUNK_SIZE 1000;

-- запуск выгрузки данных из логической таблицы sales
INSERT INTO sales_ext_download 
SELECT * FROM sales WHERE product_units > 2;
```
## 9. Удаление логических сущностей:

```sql
-- удаление внешней таблицы загрузки
DROP UPLOAD EXTERNAL TABLE sales_ext_upload;

-- удаление логического представления stores_by_sold_products
DROP VIEW stores_by_sold_products;

-- удаление внешней таблицы выгрузки
DROP DOWNLOAD EXTERNAL TABLE sales_ext_download;

-- удаление логической таблицы из всех СУБД
DROP TABLE sales.sales;

-- удаление логической базы данных
DROP DATABASE sales;
```
# Приложения

