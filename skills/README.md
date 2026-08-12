# Skills QAEF

Каталог содержит двадцать зафиксированных интерфейсов QAEF. Статус `Доступен` означает только наличие публичного определения skill; он не является evidence выполнения пользовательской задачи. Все двадцать интерфейсов опубликованы: их определения находятся в `skills/qaef-*/SKILL.md` и входят в `MANIFEST.md`. Опубликованность интерфейса не доказывает, что вызов, исполнение или результат уже проверены в вашей среде.

## Старт и контекст

| Skill | Статус | Результат |
|---|---|---|
| [`$qaef-init`](qaef-init/SKILL.md) | Доступен | Фиксирует режим, задачу, scope, authority и начальный контракт. |
| [`$qaef-map-framework`](qaef-map-framework/SKILL.md) | Доступен | Создаёт доказанную карту существующего фреймворка до правки тестов. |
| [`$qaef-context`](qaef-context/SKILL.md) | Доступен | Собирает минимальный context packet с provenance и `UNKNOWN`. |
| [`$qaef-status`](qaef-status/SKILL.md) | Доступен | Показывает состояние контекста и одно обоснованное следующее действие. |

## Проектирование

| Skill | Статус | Результат |
|---|---|---|
| [`$qaef-design-cases`](qaef-design-cases/SKILL.md) | Доступен | Проектирует трассируемые тестовые сценарии и матрицу покрытия. |
| [`$qaef-coverage`](qaef-coverage/SKILL.md) | Доступен | Находит пробелы покрытия в подтверждённой границе задачи. |
| [`$qaef-evaluate`](qaef-evaluate/SKILL.md) | Доступен | Оценивает качество дизайна по rubric и контрпримерам. |
| [`$qaef-present`](qaef-present/SKILL.md) | Доступен | Готовит русскоязычный материал для обсуждения решения с руководителем. |

## Написание тестов

| Skill | Статус | Результат |
|---|---|---|
| [`$qaef-write-unit-tests`](qaef-write-unit-tests/SKILL.md) | Доступен | Пишет isolated unit tests в стеке целевого проекта. |
| [`$qaef-write-contract-tests`](qaef-write-contract-tests/SKILL.md) | Доступен | Пишет schema и contract tests по подтверждённому контракту. |
| [`$qaef-write-api-tests`](qaef-write-api-tests/SKILL.md) | Доступен | Пишет API tests для публичного поведения. |
| [`$qaef-write-integration-tests`](qaef-write-integration-tests/SKILL.md) | Доступен | Пишет integration tests на границах зависимостей. |
| [`$qaef-write-ui-tests`](qaef-write-ui-tests/SKILL.md) | Доступен | Пишет UI component или end-to-end tests по задаче. |
| [`$qaef-write-load-tests`](qaef-write-load-tests/SKILL.md) | Доступен | Пишет load/performance profiles в выбранном проектом инструменте. |
| [`$qaef-write-security-tests`](qaef-write-security-tests/SKILL.md) | Доступен | Пишет security negative tests в границах разрешённого scope. |

## Качество и восстановление

| Skill | Статус | Результат |
|---|---|---|
| [`$qaef-review-tests`](qaef-review-tests/SKILL.md) | Доступен | Возвращает проверяемые находки с severity и предложенным исправлением. |
| [`$qaef-debug-tests`](qaef-debug-tests/SKILL.md) | Доступен | Исследует причину сбоя по evidence, не подменяя диагноз догадкой. |
| [`$qaef-triage`](qaef-triage/SKILL.md) | Доступен | Классифицирует сбой и назначает следующий безопасный шаг. |
| [`$qaef-gate`](qaef-gate/SKILL.md) | Доступен | Сводит применимые evidence перед передачей на review. |

## Передача

| Skill | Статус | Результат |
|---|---|---|
| [`$qaef-handoff`](qaef-handoff/SKILL.md) | Доступен | Передаёт изменения, evidence, статус и открытые риски следующему владельцу. |
