---
layout: default
title: INSERT INTO download_external_table
nav_order: 26
parent: Запросы SQL+
grand_parent: Справочная информация
has_children: false
has_toc: false
---

# INSERT INTO download_external_table

Запрос позволяет выгрузить данные, выбранные [SELECT](../SELECT/SELECT.md)-подзапросом 
к [логической базе данных](../../../overview/main_concepts/logical_db/logical_db.md), 
во внешний приемник данных. Запрос обрабатывается в порядке, описанном в разделе 
[Порядок обработки запросов на выгрузку данных](../../../overview/interactions/download_processing/download_processing.md).

**Примечание:** выгрузка данных возможна из [логических таблиц](../../../overview/main_concepts/logical_table/logical_table.md)
и [логических представлений](../../../overview/main_concepts/logical_view/logical_view.md).
Выгрузка данных из [материализованных представлений](../../../overview/main_concepts/materialized_view/materialized_view.md)
недоступна.

В ответе возвращается:
*   пустой объект ResultSet при успешном выполнении запроса;
*   исключение при неуспешном выполнении запроса.

При успешном выполнении запроса данные выгружаются из СУБД [хранилища](../../../overview/main_concepts/data_storage/data_storage.md), 
выбранной для выгрузки в [конфигурации](../../../maintenance/configuration/configuration.md) системы. 
Данные выгружаются в том формате и в тот приемник данных, которые были указаны 
[при создании внешней таблицы выгрузки](../CREATE_DOWNLOAD_EXTERNAL_TABLE/CREATE_DOWNLOAD_EXTERNAL_TABLE.md). Формат данных соответствует 
описанному в разделе [Формат выгрузки данных](../../download_format/download_format.md).

**Примечания:**

*   Перед выполнением запроса необходимо создать [внешнюю таблицу](../../../overview/main_concepts/external_table/external_table.md) 
    с указанием [пути](../../path_to_kafka_topic/path_to_kafka_topic.md) 
    к топику Kafka. Подробнее о порядке выполнения действий для выгрузки данных см. в разделе 
    [Выгрузка данных](../../../working_with_system/data_download/data_download.md).
*   Имена и порядок следования столбцов должны совпадать в SELECT-подзапросе на выгрузку данных и 
    внешней таблице выгрузки.

## Синтаксис

```sql
INSERT INTO [db_name.]ext_table_name SELECT ...
```

## Параметры

*   `db_name` — имя логической базы данных, из которой выгружаются данные. Указывается опционально, 
    если выбрана логическая БД, [используемая по умолчанию](../../../working_with_system/other_features/default_db_set-up/default_db_set-up.md);
*   `ext_table_name` — имя внешней таблицы выгрузки.

## Ограничения

*   Секция [DATASOURCE_TYPE](../SELECT/SELECT.md#param_datasource_type) игнорируется в SELECT-подзапросе.
*   Выгружаемые данные должны быть доступны в СУБД, выбранной для выгрузки в конфигурации системы.

## Пример

```sql
INSERT INTO sales.sales_ext_download
SELECT * FROM sales.sales WHERE sales.product_units > 2
```