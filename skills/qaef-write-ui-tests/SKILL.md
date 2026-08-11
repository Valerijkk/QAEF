---
name: qaef-write-ui-tests
description: Использовать, когда нужно создать UI component или end-to-end тест пользовательского сценария
---

# Написание UI-тестов

## Назначение

Сохранять нативные UI component или end-to-end тесты существенного пользовательского поведения. Реализация требует реальных test-файлов и разрешённого test-support в целевом проекте.

## Входы

Получить `objective`, один `mode`, буквальные paths, UI behavior, user-visible oracle, accessibility semantics, requirement/risk/TC либо отсутствие ссылки, target stack, нативные fixtures/representative tests, допустимые данные, `authority`, `allowed_tools` и получателя handoff. Указать provenance/freshness либо `UNKNOWN`.

## Режим

В `existing` сохранять доказанные stack/layout/fixtures/pattern/command; в `greenfield` — утверждённый минимальный vertical path выбранного стека. Переход `existing` ↔ `greenfield` требует отдельного явного решения пользователя. Выбирать component test для границы компонента, end-to-end — для сквозного пути. Не создавать runner/config/dependency.

## Полномочия

Записывать только буквальные test-пути и разрешённый test-support; production-код исключён. Execute/egress требуют отдельных полномочий. Использовать минимальные синтетические или разрешённые данные; не сохранять secrets, tokens, PII и чувствительные raw payloads. Screenshots/traces допустимы только при поддержке stack, полномочиях и sanitization; oracle они не заменяют.

## Порядок работы

1. Связать сценарий с requirement/risk/TC либо отметить отсутствие ссылки. Проверить применимость классов `happy`, `negative`, `authn`, `authz`, `boundary`, `edge`, `corner` и `failure-recovery`; выбрать только обоснованные классы и существенный user-visible oracle, не только отсутствие ошибки, DOM existence или внутренний event.
2. Задать детерминированное начало, уникальные test-owned data и независимость от порядка. Cleanup созданных ресурсов выполнять идемпотентно в `finally`, ограничивая ресурсами теста.
3. Для каждого элемента заново выбирать locator: role (с accessible name, когда применим; для `role=status` name не обязателен) → видимый text → test-id. Переходить дальше только когда предыдущий уровень неприменим. CSS допустим лишь когда неприменимы все три уровня и есть evidence стабильности selector.
4. Использовать native auto-wait или наблюдаемое условие: visible/enabled state, URL, связанный response/state transition. Не использовать fixed sleep (`waitForTimeout`, `sleep`, `delay`).
5. Сохранить минимальный нативный test. Page/component abstraction создавать только по доказанному reuse или существующей convention; один use reuse не доказывает.
6. Выполнять только доказанную native command при `execute=true`. Без execution сохранить `executed_commands: []`, `execution_status: NOT_RUN`, `runtime_behavior: UNKNOWN` и `assertion_sensitivity: NOT_RUN`.

## Результат

Вернуть saved paths, слой/pattern, trace, state/cleanup, locator/oracle rationale, `changed_paths`, `executed_commands`, статусы, evidence, risks и next action. Checklist или несохранённый snippet не являются реализацией.

## Stop conditions

Вернуть `BLOCKED`, если нет mode, literal write authority/path, UI contract, user-visible oracle, target stack/pattern или безопасного state/cleanup. Вернуть `UNKNOWN`, если product truth, semantics, provenance или runtime behavior не подтверждены. Без execute-authority запись разрешена, выполнение — `NOT_RUN`; command и `PASS` не изобретать.

## Evidence

Для существенных assertions назвать observable product break и доказать чувствительность RED, controlled mutation либо negative sentinel; незапущенное — `NOT_RUN`. Зафиксировать locator priority per element и fallback. Static diff доказывает сохранение, не runtime correctness. `PASS` требует evidence каждого acceptance-критерия.

## Ошибки применения

Не пропускать доступный role или text ради test-id; не требовать accessible name, когда он неприменим, включая `role=status`; не брать CSS без исчерпания трёх уровней и evidence стабильности. Запрещены fixed sleep, internal-only oracle, shared state, happy-path-only cleanup, одноразовая abstraction без convention, новый runner/config, production fix и выдуманный runtime `PASS`.

## Handoff

Передать reviewer/verifier exact paths, UI contract/trace, слой/pattern, locator decisions, state/cleanup, oracles, privacy-safe evidence, честные `NOT_RUN`/`UNKNOWN`/`BLOCKED`, risks и next action. Product defect передать владельцу без production fix.
