---
name: qaef-design-cases
description: Использовать, когда нужно превратить требования и риски в проверяемые тест-кейсы перед автоматизацией
---

# Проектирование тест-кейсов

## Назначение

Создать ограниченный, трассируемый `test-plan` и набор `test-case` для одной наблюдаемой границы без реализации тестового или production-кода.

## Входы

Прочитать task contract, context packet, requirement/risk/TC, accepted framework map или blueprint и [test-plan](../../templates/test-plan.md) с [test-case](../../templates/test-case.md). Нужны literal `owned_paths`, exclusions, provenance и acceptance. Если схемы TC нет, записать это как `UNKNOWN`, не изобретать ID.

## Режим

Выбрать ровно один `existing` или `greenfield`. В `existing` сохранить принятую карту, нативные слои и пути. В `greenfield` использовать выбранный stack и approved blueprint первой вертикали. Смена режима требует решения пользователя.

## Полномочия

Читать только назначенные источники. При `write=true` сохранять только `test-plan` и `test-case` в literal `owned_paths`. Не менять target tests, test-support, production-код или docs. Execute и egress требуют отдельных authority.

## Порядок работы

1. Разделить доказанные требования и риски на наблюдаемые границы; конфликт оставить `UNKNOWN`.
2. Построить одну bounded risk matrix: requirement/risk/TC, boundary, layer и rationale.
3. Для каждого класса `happy`, `negative`, `authn`, `authz`, `boundary`, `edge`, `corner`, `failure-recovery` записать применимость. `N/A` — только rationale, не case.
4. Для каждого включённого case указать точные известные requirement/risk/TC, детерминированные data, literal oracle, layer, automation path, production break и planned RED, controlled mutation либо negative sentinel; их статус до выполнения — `NOT_RUN`.
5. Сохранить только design artifacts в разрешённых путях. Число cases следует только из risk rationale, не из target count.

## Результат

Сохранённые `test-plan` и `test-case` artifacts с traceability matrix, обоснованными включениями и `N/A`, открытыми unknowns и маршрутом следующему writer. Несохранённый совет не является результатом.

## Stop conditions

Вернуть `BLOCKED`, если нет product truth, risk, наблюдаемой границы, map/blueprint, literal design scope, write authority или безопасных данных. Вернуть `UNKNOWN` для спорных требования, interface, command, stack или TC. Невыполненные проверки — `NOT_RUN`; не заявлять `PASS`.

## Evidence

Зафиксировать exact changed paths, risk rationale каждого case, exclusions, data/oracle/break/sensitivity plan, actual commands и их results. Без execute: `executed_commands: []`, `execution_status: NOT_RUN`.

## Ошибки применения

- Нельзя создавать TC-ID, если источник не задаёт схему или ID.
- Нельзя подменять risk rationale фиксированным числом cases или design-only записью.
- Нельзя превращать `N/A` либо `UNKNOWN` в test-case; сохранять их причину отдельно.
- Нельзя выдавать checklist, snippet или изменение target за сохранённый design.

## Handoff

Передать одному `handoff_recipient` design paths, changed paths, accepted subset, traceability matrix, applicable classes с rationale, unknowns, planned writer layer, evidence, limitations и one next action. Реализация тестов начинается отдельным writer contract.
