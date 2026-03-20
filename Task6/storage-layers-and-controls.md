# Слои хранения и меры контроля

## 1. Слои хранения

| Слой | Назначение | Доступ | Запрет |
| --- | --- | --- | --- |
| `Landing zone` | Временный прием исходных наборов | `Pre-ingest source screen`, `Schema registry`, `Classifier`, `Privacy policy gate` | Аналитические роли и экспорт запрещены |
| `Restricted zone` | Классифицированные чувствительные наборы | `Transform`, ограниченные администраторы | Нет прямого BI-доступа |
| `Quarantine` | Новые схемы, `schema drift`, наборы с низкой уверенностью, deny-cases | Ручная проверка, `SecOps`, `lineage` | Нет публикации в `Mart` |
| `Curated pseudonymized zone` | Наборы после псевдонимизации и маскирования | `Transform`, публикация в `Mart`, ограниченные pipeline-сервисы | Нет прямого экспорта без policy |
| `Mart / aggregate zone` | BI, ML, AI, отчеты | Аналитические роли и `Controlled export` | Нет прямых идентификаторов |

## 2. Правила маршрутизации

1. Набор сначала проходит `Pre-ingest source screen`.
2. `Pre-ingest source screen` проверяет регистрацию feed, transport, source-level lawful purpose и минимальную полноту subject references; исходная confidentiality-разметка от источника не обязательна.
3. Только разрешенный набор попадает в `Landing zone`.
4. При неизвестной или несовместимой схеме набор уходит в `Quarantine`.
5. Публикация в `Restricted` или `Curated` разрешена только при `consent=allow`, `legal_basis=allow`, `stop_processing=false` и полном наборе тегов.
6. Публикация в `Mart` разрешена только при `classification=pass`, `privacy_gate=pass`, `pseudonymization_or_masking=pass`.
7. Чтение `Mart` и file export выполняются только через `Read policy gate` и `Controlled export`.
8. При `stop_processing=true` или `policy=deny` публикация запрещена; набор переводится в `Quarantine` либо удаляется по delete rule.

## 3. Контроли

| Контроль | Проверяемое условие |
| --- | --- |
| `T6-01` | Каждый набор имеет запись в `Schema registry` |
| `T6-02` | Каждый набор после `Classification` и до публикации за пределы `Landing` имеет `tag.class`, `tag.domain`, `tag.subject_scope`, `tag.legal_basis`, `tag.retention`, `tag.masking`, `tag.export`, `tag.audit` |
| `T6-03` | Решение по `consent`, `legal basis`, `stop processing` берется только из канонического privacy-контура |
| `T6-04` | `Mart` не содержит прямых идентификаторов и не читается в обход `Read policy gate` |
| `T6-05` | `Quarantine` блокирует публикацию low-confidence, unknown schema и deny-case наборов |
| `T6-06` | Каждая публикация, выдача витрины и экспорт имеют запись в `lineage` |
| `T6-07` | Каждый случай deny/delete фиксируется в `lineage` и аудите |
