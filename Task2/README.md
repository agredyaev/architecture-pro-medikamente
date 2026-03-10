# Task2 — проектирование решения

## 1. Цель

Спроектировать C4 Context для MVP и target-state со встроенными privacy-механизмами в бизнес-потоках.

## 2. Что входит в MVP

- Пациент сам записывается без участия сотрудника.
- Ресепшен видит и обрабатывает запись в едином контуре.
- Врач видит свое расписание.
- Система отправляет напоминания пациенту и специалисту.
- Доступ, consent и stop processing проходят через отдельный privacy-контур.

## 3. Что добавляется в target

| Блок | Назначение |
| --- | --- |
| `Операционная платформа` | Профиль пациента, запись, медкарта, биллинг |
| `Privacy и access control` | Consent, policy, tagging, lifecycle, controlled export |
| `Аналитическая платформа` | Classification, pseudonymization, BI, ML, AI |
| `Аудит и аномалии` | Audit trail, monitoring, anomaly detection |
| `Лаборатория` | Контрактный обмен назначениями и результатами |
| `Legacy 1С` | Переходный финансовый, складской и supplier-domain контур до выделения отдельного procurement-domain |

## 4. Нотация и состав диаграмм

Диаграммы выполнены в PlantUML (C4 Context, стиль `yandex_c4_style.puml`).

| Файл | Уровень | Состав |
| --- | --- | --- |
| `context-mvp-to-be.puml` | C4 Context — MVP | 3 актора (`Пациент`, `Ресепшен`, `Врач`), 1 основная система (`Медикаменте MVP`), 1 внешняя система (`Уведомления`) |
| `context-target-context.puml` | C4 Context — target | 6 акторов, 4 системы (`Операционная платформа`, `Privacy и access control`, `Аналитическая платформа`, `Аудит и аномалии`), 4 внешних системы (`Лаборатория`, `Legacy 1С`, `Уведомления`, `BI / ML / AI`) |

## 5. Проверяемые условия

| ID | Правило | Где закрыто |
| --- | --- | --- |
| `MVP-01` | Пациент создает запись без участия сотрудника | `Task2/context-mvp-to-be.puml` |
| `MVP-02` | Ресепшен видит запись и работает с ней в едином контуре | `Task2/context-mvp-to-be.puml` |
| `MVP-03` | Система отправляет напоминание за день до приема | `Task2/context-mvp-to-be.puml` |
| `TGT-01` | Пациент видит только собственные данные | `Task2/context-target-context.puml` |
| `TGT-02` | Отзыв согласия и stop processing выполняются в целевом контуре | `Task2/context-target-context.puml` |
| `TGT-03` | Аналитика использует только разрешенные наборы | `Task2/context-target-context.puml` |
| `TGT-04` | Подтверждение и отмена записи проходят через целевой контур уведомлений | `Task2/context-target-context.puml` |
| `TGT-05` | Analytics проверяет policy, consent, legal basis и stop processing через privacy-контур | `Task2/context-target-context.puml` |
