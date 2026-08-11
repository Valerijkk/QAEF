# Команды QAEF

Все команды выбираются после явного решения `existing | greenfield`. Результат каждой команды должен содержать evidence, неизвестное и условия остановки.

## Старт и контекст

| Skill | Результат |
|---|---|
| `$qaef-init` | Фиксирует режим, задачу, scope, authority и начальный контракт. |
| `$qaef-map-framework` | Создаёт доказанную карту существующего фреймворка до правки тестов. |
| `$qaef-context` | Собирает минимальный context packet с provenance и `UNKNOWN`. |
| `$qaef-status` | Показывает состояние контекста и одно обоснованное следующее действие. |

## Проектирование

| Skill | Результат |
|---|---|
| `$qaef-design-cases` | Проектирует трассируемые тестовые сценарии и матрицу покрытия. |
| `$qaef-coverage` | Находит пробелы покрытия в подтверждённой границе задачи. |
| `$qaef-evaluate` | Оценивает качество дизайна по rubric и контрпримерам. |
| `$qaef-present` | Готовит русскоязычный материал для обсуждения решения с руководителем. |

## Написание тестов

При write-authority эти skills создают или меняют реальный тестовый код и явно разрешённый test-support в целевом репозитории.

| Skill | Результат |
|---|---|
| `$qaef-write-unit-tests` | Пишет isolated unit tests в стеке целевого проекта. |
| `$qaef-write-contract-tests` | Пишет schema и contract tests по подтверждённому контракту. |
| `$qaef-write-api-tests` | Пишет API tests для публичного поведения. |
| `$qaef-write-integration-tests` | Пишет integration tests на границах зависимостей. |
| `$qaef-write-ui-tests` | Пишет UI component или end-to-end tests по задаче. |
| `$qaef-write-load-tests` | Пишет load/performance profiles в выбранном проектом инструменте. |
| `$qaef-write-security-tests` | Пишет security negative tests в границах разрешённого scope. |

## Качество и восстановление

| Skill | Результат |
|---|---|
| `$qaef-review-tests` | Возвращает проверяемые находки с severity и предложенным исправлением. |
| `$qaef-debug-tests` | Исследует причину сбоя по evidence, не подменяя диагноз догадкой. |
| `$qaef-triage` | Классифицирует сбой и назначает следующий безопасный шаг. |
| `$qaef-gate` | Сводит применимые evidence перед передачей на review. |

## Передача

| Skill | Результат |
|---|---|
| `$qaef-handoff` | Передаёт изменения, evidence, статус и открытые риски следующему владельцу. |

Контракт продукта — в [QAEF.md](QAEF.md); выбор первого маршрута — в [START-HERE.md](START-HERE.md).
