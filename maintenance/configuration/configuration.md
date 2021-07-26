---
layout: default
title: Конфигурация
nav_order: 2
parent: Эксплуатация
has_children: false
---

# Конфигурация {#configuration}
{: .no_toc }

<details markdown="block">
  <summary>
    Содержание раздела
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

Конфигурация системы задается в текстовом файле YAML-формата. Параметры конфигурации организованы 
в иерархическую структуру типа дерево.

В разделе представлен примеры актуальных файлов конфигурации системы: [конфигурации сервиса исполнения 
запросов](#core_configuration) и [конфигурации сервиса мониторинга статусов Kafka](#status_monitor_configuration). 
Перед каждым параметром указан комментарий, поясняющий назначение этого параметра. Для наглядности конфигурация сервиса исполнения
запросов параметры разделена на отдельные секции.

## Конфигурация сервиса исполнения запросов {#core_configuration}

### Настройки журналирования {#logging_parameters}
``` yaml
# раздел настроек журналирования
logging:
# задание уровня важности сообщений, журналируемых в лог-файле
  level:
io.arenadata.dtm.query.execution: TRACE
```

### Настройки управления DTM {#dtm_parameters}
``` yaml
# раздел настроек управления DTM
management:
# номер порта сервиса метрик
  server:
    port: ${DTM_METRICS_PORT:8080}
# настройки конечных точек ADTM
  endpoints:
# настройка генерации метрик со стороны ADTM
    enabled-by-default: ${DTM_METRICS_ENABLED:true}
# настройка видимости метрик через веб-соединения
    web:
      exposure:
# состав метрик, видимых через веб-соединения
        include: ${DTM_METRICS_SCOPE:info, health, requests}
```

### Настройки сервиса исполнения запросов {#core_parameters}
``` yaml
# раздел настроек сервиса исполнения запросов
core:
# настройки плагинов
  plugins:
# список работающих плагинов к соответствующим СУБД
    active: ${CORE_PLUGINS_ACTIVE:ADG, ADB, ADQM}
# настройки профилей приоритетности СУБД по категориям SQL-запросов
    category:
      mapping:
# профиль для общих реляционных запросов
        RELATIONAL: ${DTM_CORE_PLUGINS_RELATIONAL:ADB, ADQM, ADG}
# профиль для запросов аналитики
        ANALYTICAL: ${DTM_CORE_PLUGINS_ANALYTICAL:ADQM, ADB, ADG}
# профиль для запросов ключ-значение
        DICTIONARY: ${DTM_CORE_PLUGINS_DICTIONARY:ADG, ADB, ADQM}
# профиль для других категорий запросов
        UNDEFINED:  ${DTM_CORE_PLUGINS_UNDEFINED:ADB, ADQM, ADG}
# настройки сетевых подключений через HTTP-протокол
  http:
# номер порта сервиса исполнения запросов
    port: ${DTM_CORE_HTTP_PORT:9090}
# настройка режима оптимизации работы сокета TCP_NODELAY
    tcpNoDelay: ${DTM_CORE_HTTP_TCP_NO_DELAY:true}
# настройка режима TCP FAST_OPEN
    tcpFastOpen: ${DTM_CORE_HTTP_TCP_FAST_OPEN:true}
# настройка режима оптимизации работы сокета TCP_QUICKACK
    tcpQuickAck: ${DTM_CORE_HTTP_TCP_QUICK_ACK:true}
# настройки окружения
  env:
# имя окружения для формирования полных имен логических БД
    name: ${DTM_NAME:test}
# настройки временной зоны
  settings:
timeZone: ${CORE_TIME_ZONE:UTC}
# настройки синхронизации материализованных представлений
  matviewsync:
# периодичность запуска синхронизации в миллисекундах
    periodMs: ${MATERIALIZED_VIEWS_SYNC_PERIOD_MS:5000}
# максимальное количество попыток синхронизации представления, после рестарта системы счетчик обнуляется
    retryCount: ${MATERIALIZED_VIEWS_RETRY_COUNT:10}
# максимальное количество представлений, синхронизируемых одновременно     
    maxConcurrent: ${MATERIALIZED_VIEWS_CONCURRENT:2}
# настройки генерации метрики сервиса исполнения запросов
  metrics:
    isEnabled: ${DTM_CORE_METRICS_ENABLED:true}
# настройки источника данных
  datasource:
# настройки для EDML-операторов
    edml:
# тип СУБД-источника (ADB, ADQM, ADG)
      sourceType: ${EDML_DATASOURCE:ADB}
# количество записей, по умолчанию выгружаемых в одном сообщении топика Каfka
      defaultChunkSize: ${EDML_DEFAULT_CHUNK_SIZE:1000}
# период проверки статуса плагина в миллисекундах
      pluginStatusCheckPeriodMs: ${EDML_STATUS_CHECK_PERIOD_MS:1000}
# интервал времени ожидания (в миллисекундах) до тайм-аута при работе с первым смещением в топике Kafka
      firstOffsetTimeoutMs: ${EDML_FIRST_OFFSET_TIMEOUT_MS:15000}
# интервал времени ожидания (в миллисекундах) до тайм-аута при ожидании смены смещения в топике Kafka
      changeOffsetTimeoutMs: ${EDML_CHANGE_OFFSET_TIMEOUT_MS:10000}
# настройки Zookeeper
    zookeeper:
# сетевой адрес хоста Zookeeper для служебной БД
      connection-string: ${ZOOKEEPER_DS_ADDRESS:10.92.3.47}
# интервал времени ожидания (в миллисекундах) соединения с хостом Zookeeper для служебной БД до достижения тайм-аута
      connection-timeout-ms: ${ZOOKEEPER_DS_CONNECTION_TIMEOUT_MS:30000}
# интервал времени бездействия (в миллисекундах) в сессии хоста Zookeeper для служебной БД до достижения тайм-аута
      session-timeout-ms: ${ZOOKEEPER_DS_SESSION_TIMEOUT_MS:86400000}
# корневой путь к хосту Zookeeper для служебной БД
      chroot: ${ZOOKEEPER_DS_CHROOT:/adtm}
# настройки взаимодействия сервиса исполнения запросов с брокером сообщений Kafka
  kafka:
    producer:
      property:
# указание сериализатора строковых ключей
        key.serializer: org.apache.kafka.common.serialization.StringSerializer
# указание сериализатора строковых значений
        value.serializer: org.apache.kafka.common.serialization.StringSerializer
# настройка кластера Zookeeper для взаимодействия с брокером сообщений Kafka
    cluster:
      zookeeper:
# сетевой адрес хоста Zookeeper для брокера сообщений Kafka
        connection-string: ${ZOOKEEPER_KAFKA_ADDRESS:10.92.3.47}
# интервал времени ожидания (в миллисекундах) соединения с хостом Zookeeper для брокера сообщений Kafka до достижения тайм-аута
        connection-timeout-ms: ${ZOOKEEPER_KAFKA_CONNECTION_TIMEOUT_MS:30000}
# интервал времени бездействия (в миллисекундах) в сессии хоста Zookeeper для брокера сообщений Kafka до достижения тайм-аута
        session-timeout-ms: ${ZOOKEEPER_KAFKA_SESSION_TIMEOUT_MS:86400000}
# корневой путь к хосту Zookeeper для брокера сообщений Kafka
        chroot: ${ZOOKEEPER_KAFKA_CHROOT:}
# настройки администратора Kafka
    admin:
# интервал времени ожидания (в миллисекундах) входного потока данных для брокера сообщений Kafka до достижения тайм-аута
      inputStreamTimeoutMs: ${KAFKA_INPUT_STREAM_TIMEOUT_MS:2000}
# настройки статусов публикации событий брокером сообщений Kafka
    status.event.publish:
# разрешение на публикацию событий
      enabled: ${KAFKA_STATUS_EVENT_ENABLED:false}
# наименование топика Kafka, в который публикуются события
      topic: ${KAFKA_STATUS_EVENT_TOPIC:status.event}
# настройки подключения к сервису мониторинга статусов Kafka 
    statusMonitor:
# сетевой адрес и путь для получения информации о статусе сервиса
      statusUrl: ${STATUS_MONITOR_URL:http://127.0.0.1:9095/status}
# сетевой адрес и путь для получения информации о версии сервиса
      versionUrl: ${STATUS_MONITOR_VERSION_URL:http://127.0.0.1:9095/versions}
# настройки при использовании фреймворка vertx
  vertx:
    pool:   
# максимальный размер пула потоков, выполняющих долгие операции
      worker-pool: ${DTM_CORE_WORKER_POOL_SIZE:20}
# максимальный размер пула потоков, обрабатывающих события vertx
      event-loop-pool: ${DTM_CORE_EVENT_LOOP_POOL_SIZE:20}    
# максимальный объем пула задач в сервисе исполнения запросов
      task-pool: ${DTM_CORE_TASK_POOL_SIZE:20}
# интервал времени завершения задачи, выполняемой в сервисе исполнения запросов
      task-timeout: ${DTM_CORE_TASK_TIMEOUT:86400000}
# настройки кэширования запросов
  cache:
# начальная емкость кэша
    initialCapacity: ${CACHE_INITIAL_CAPACITY:100000}
# максимальный размер кэша
    maximumSize: ${CACHE_MAXIMUM_SIZE:100000}
# время (в минутах) устаревания кэша после последнего момента обращения к нему
    expireAfterAccessMinutes: ${CACHE_EXPIRE_AFTER_ACCESS_MINUTES:99960}
# настройки отката дельты
  delta:
# периодичность проверки операций записи, требующих остановки, в миллисекундах 
    rollback-status-calls-ms: ${DELTA_ROLLBACK_STATUS_CALLS_MS:2000}      
```

### Настройки СУБД ADB {#adb_parameters}
``` yaml
# настройки ADB
adb:
# настройки источника данных ADB
  datasource:
# имя пользователя/логин для авторизации в ADB
    user: ${ADB_USERNAME:dtm}
# пароль для авторизации в ADB
    password: ${ADB_PASS:dtm}
# сетевой адрес хоста с ADB
    host: ${ADB_HOST:10.92.3.105}
# сетевой адрес порта на хосте с ADB
    port: ${ADB_PORT:5432}
# максимальное количество подключений к ADB в одном потоке; 
# максимальное количество подключений к ADB в целом по всем потокам равно произведению poolSize и executorsCount 
    poolSize: ${ADB_MAX_POOL_SIZE:3}
# количество одновременных потоков, исполняющих запросы к ADB
    executorsCount: ${ADB_EXECUTORS_COUNT:3}
# максимальный размер результата, возвращаемого по FETCH-запросу к ADB
    fetchSize: ${ADB_FETCH_SIZE:1000}
# настройки механизма загрузки данных в ADB
  mppw:
# наименование консьюмер-группы ADB для взаимодействия с брокером сообщений Kafka
    consumerGroup: ${ADB_LOAD_GROUP:adb-emulator-load-adb}
# максимальный размер пула подключений к ADB для операций загрузки
    poolSize: ${ADB_MPPW_POOL_SIZE:2}
# значение тайм-аута ожидания (в миллисекундах) для остановки загрузки
    stopTimeoutMs: ${ADB_MPPW_STOP_TIMEOUT_MS:86400000}
# предельное количество сообщений для операции загрузки в ADB
    defaultMessageLimit: ${ADB_MPPW_DEFAULT_MESSAGE_LIMIT:100}
# значение тайм-аута ожидания (в миллисекундах) для FDW-коннектора ADB
    fdwTimeoutMs: ${ADB_MPPW_FDW_TIMEOUT_MS:1000}
# признак использования исторических таблиц
  with-history-table: ${ADB_WITH_HISTORY_TABLE:false}
```

### Настройки СУБД ADG {#adg_parameters}
``` yaml
# настройки ADG
adg:
  tarantool:
    db:
# сетевой адрес хоста с ADG
      host: ${TARANTOOL_DB_HOST:10.92.3.120}
# сетевой адрес порта на хосте с ADG
      port: ${TARANTOOL_DB_PORT:3306}
# имя пользователя/логин для авторизации в ADG
      user: ${TARANTOOL_DB_USER:admin}
# пароль для авторизации в ADG
      password: ${TARANTOOL_DB_PASS:memstorage-cluster-cookie}
# максимальный интервал времени ожидания выполнения операции ADG до тайм-аута
      operationTimeout: ${TARANTOOL_DB_OPER_TIMEOUT:60000}
# максимальное количество повторных попыток выполнения операции
      retryCount: ${TARANTOOL_DB_RETRY_COUNT:0}
# движок ADG
      engine: ${TARANTOOL_DEFAULT_ENGINE:MEMTX}
# настройки картриджа Tatantool
    cartridge:
# сетевой путь и порт к картриджу Tarantool
      url: ${TARANTOOL_CATRIDGE_URL:http://10.92.3.120:8086}
# настройки механизма загрузки данных
  mppw:
# наименование консьюмер-группы ADG для взаимодействия с брокером сообщений Kafka
    consumerGroup: ${ADG_CONSUMER_GROUP:tarantool-group-csv}
    kafka:
# максимальное количество сообщений в топике Kafka на раздел ADG
      maxNumberOfMessagesPerPartition: ${ADG_MAX_MSG_PER_PARTITION:200}
# время простоя (в секундах) callback-функции
      callbackFunctionSecIdle: ${ADG_CB_FUNC_IDLE:100}
# настройки отката операции
  rollback:
# размер пакета операций при откате
    eraseOperationBatchSize: ${ADG_ROLLBACK_OPERATION_BATCH_SIZE:300}
# настройки отказоустойчивости ADG по паттерну circuitbreaker
  circuitbreaker:
# максимальное количество отказов ADG
    maxFailures: ${ADG_CIRCUIT_BREAKER_MAX_FAILURES:5}
# интервал времени фиксации отказа при пропадании отклика ADG
    timeout: ${ADG_CIRCUIT_BREAKER_TIMEOUT:30000}
# использование паттерна fallback при отказе
    fallbackOnFailure: ${ADG_CIRCUIT_BREAKER_FALLBACK_ON_FAILURE:false}
# интервал времени до сброса по паттерну timeout
    resetTimeout: ${ADG_CIRCUIT_BREAKER_RESET_TIMEOUT:10000}
# настройки для подключений веб-клиентов
  web-client:
# максимальный размер пула подключений веб-клиентов к ADG
    max-pool-size: ${ADG_WEB_CLIENT_MAX_POOL_SIZE:100}
```

### Настройки СУБД ADQM {#adqm_parameters}
``` yaml
# настройки ADQM
adqm:
# настройка источника данных ADQM
  datasource:
# наименование ADQM
    database: ${ADQM_DB_NAME:upload}
# имя пользователя/логин для авторизации в ADQM
    user: ${ADQM_USERNAME:}
# пароль для авторизации в ADQM
    password: ${ADQM_PASS:}
# сетевой адрес хоста с ADQM и номер порта на хосте
    hosts: ${ADQM_HOSTS:10.92.3.30:8123}
# интервал времени ожидания отклика соединения с ADQM до тайм-аута
    socketTimeout: ${ADQM_SOCKET_TIMEOUT:30000}
# интервал времени ожидания завершения обмена данными с ADQM до тайм-аута
    dataTransferTimeout: ${ADQM_DATA_TRANSFER_TIMEOUT:10000}
# настройки DDL-операторов
  ddl:
# наименование кластера ADQM
    cluster: ${ADQM_CLUSTER:test_arenadata}
# настройки механизма выгрузки данных из ADQM
  mppr:
# сетевой адрес и путь для запросов на выгрузку данных
    loadingUrl: ${ADQM_MPPR_CONNECTOR_URL:http://10.92.3.14:8086/query}
# сетевой адрес и путь для получения информации о версии коннектора
    versionUrl: ${ADQM_MPPR_CONNECTOR_VERSION_URL:http://10.92.3.14:8086/versions}
# настройки механизма загрузки данных ADQM
  mppw:
# наименование консьюмер-группы ADQM для загрузки данных в ADQM
# не используется
    consumerGroup: ${ADQM_CONSUMER_GROUP:adqm}
# сетевой адрес брокера сообщений Kafka
    kafkaBrokers: ${ADQM_BROKERS:10.92.3.31:9092}
# тип интерфейса для загрузки данных в ADQM
    loadType: ${ADQM_MPPW_LOAD_TYPE:REST}
# сетевой адрес и путь к REST-интерфейсу для загрузки новых данных в ADQM
    restStartLoadUrl: ${ADQM_REST_START_LOAD_URL:http://10.92.3.86:8090/newdata/adqm/start}
# сетевой адрес и путь к REST-интерфейсу для остановки загрузки данных в ADQM
    restStopLoadUrl: ${ADQM_REST_STOP_LOAD_URL:http://10.92.3.86:8090/newdata/adqm/stop}
# сетевой адрес и путь для получения информации о версии коннектора
    versionUrl: ${ADQM_MPPW_CONNECTOR_VERSION_URL:http://10.92.3.86:8090/versions}
# наименование коньсюмер-группы для загрузки данных в ADQM через REST API
    restLoadConsumerGroup: ${ADQM_REST_LOAD_GROUP:adb-emulator-load-adqm}
# настройки для подключений веб-клиентов
  web-client:
# максимальный размер пула подключений веб-клиентов к ADQM
    max-pool-size: ${ADQM_WEB_CLIENT_MAX_POOL_SIZE:100}
```

## Конфигурация сервиса мониторинга статусов Kafka {#status_monitor_configuration}

``` yaml
# настройки cервиса мониторинга статусов Kafka
monitor:
# список адресов брокеров сообщений Kafka
brokersList: ${STATUS_MONITOR_BROKERS:10.92.3.31:9092}
# количество потребителей (консьюмеров) cервиса мониторинга Kafka
consumersCount: ${STATUS_MONITOR_CONSUMERS:8}
```