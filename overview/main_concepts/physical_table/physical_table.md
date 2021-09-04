---
layout: default
title: Физическая таблица
nav_order: 11
parent: Основные понятия
grand_parent: Обзор понятий, компонентов и связей
has_children: false
has_toc: false
---

# Физическая таблица {#physical_table}

_Физическая таблица_ — таблица СУБД [хранилища](../data_storage/data_storage.md), каждая запись 
которой описывает состояние объекта [логической таблицы](../logical_table/logical_table.md) 
или [материализованного представления](../materialized_view/materialized_view.md) 
в определенный период времени. В зависимости от [типа физической таблицы](../physical_schema/physical_schema.md) 
состояние ее объектов может быть новым ("горячим"), актуальным или архивным.

Подробнее о переходе данных из одного состояние в другое см. в разделе 
[Версионирование данных](../../../working_with_system/data_upload/data_versioning/data_versioning.md).