---
layout: default
title: Порядок перезапуска системы
nav_order: 6
parent: Связи с другими системами и компонентами
grand_parent: Обзор понятий, компонентов и связей
has_children: false
has_toc: false
---

# Порядок перезапуска системы {#restart_processing}

При перезапуске системы выполняются следующие действия:
1. Возобновляются незавершенные [операции записи](../../main_concepts/write_operation/write_operation.md): по операциям 
   в статусе «Ошибка» запускается отмена, по операциям в статусе «Выполняется» — загрузка данных 
   в [СУБД](../../../introduction/supported_DBMS/supported_DBMS.md) 
   [хранилища](../../main_concepts/data_storage/data_storage.md). При этом возобновляются все незавершенные операции, 
   кроме операций [обновления данных](../../../working_with_system/data_update/data_update.md) со статусом «Выполняется».
2. Запускается синхронизация [материализованных представлений](../../main_concepts/materialized_view/materialized_view.md) 
   [окружения](../../main_concepts/environment/environment.md), если она включена в 
   [конфигурации системы](../../../maintenance/configuration/system/system.md).
   
Синхронизацией материализованных представлений можно управлять с помощью параметра конфигурации 
`MATERIALIZED_VIEWS_SYNC_PERIOD_MS`.
{: .note-wrapper}