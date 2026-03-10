# Task6 — движок классификации данных

## 1. Цель

Спроектировать C2-диаграмму движка классификации перед загрузкой в аналитическое хранилище, включая обработку неразмеченных и изменяющихся схем, а также метрики качества и масштабирования.

## 2. Что показывает решение

- `Pre-ingest source screen` до попадания набора в аналитические слои: проверяет `feed registration`, `transport` и `source-level lawful purpose`, без требования confidentiality-разметки на входе.
- Явные слои `Landing`, `Restricted`, `Curated`, `Mart` и отдельный `Quarantine`.
- `Schema registry` и обработка `schema drift`.
- `Privacy policy gate`, `Controlled export`, аудит и `lineage`.

`Landing zone` — временный изолированный буфер приема, не аналитическое хранилище.

Канонический `source of truth` для `consent`, `legal basis` и `stop processing` расположен во внешнем privacy-контуре; движок обращается к нему через `Privacy policy gate`.