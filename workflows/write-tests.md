# Написание тестов

## Результат

При явной write-authority сохранить реальный test-код и только необходимый разрешённый test-support в целевом репозитории. Advice, checklist, псевдокод и несохранённый snippet не являются результатом задачи на реализацию.

## Ровно семь маршрутов

Все семь writer skills **планируются и ещё недоступны (`PLANNED / NOT_YET_AVAILABLE`)**. Таблица фиксирует будущую маршрутизацию, а не заявляет возможность вызова.

| Наблюдаемая граница | Единственный будущий маршрут | Текущий статус |
|---|---|---|
| Изолированная единица кода | `$qaef-write-unit-tests` | `PLANNED / NOT_YET_AVAILABLE` |
| Опубликованное соглашение между сторонами | `$qaef-write-contract-tests` | `PLANNED / NOT_YET_AVAILABLE` |
| Публичное API-поведение | `$qaef-write-api-tests` | `PLANNED / NOT_YET_AVAILABLE` |
| Граница с хранилищем, очередью, сервисом или зависимостью | `$qaef-write-integration-tests` | `PLANNED / NOT_YET_AVAILABLE` |
| UI-компонент или пользовательский поток | `$qaef-write-ui-tests` | `PLANNED / NOT_YET_AVAILABLE` |
| Согласованный профиль нагрузки | `$qaef-write-load-tests` | `PLANNED / NOT_YET_AVAILABLE` |
| Разрешённая негативная security-граница | `$qaef-write-security-tests` | `PLANNED / NOT_YET_AVAILABLE` |

Других `$qaef-write-*` маршрутов этот workflow не определяет. Если задача содержит независимые слои, разделите её на отдельные ограниченные контракты; не используйте catch-all writer.

Пока выбранный skill отсутствует, можно зафиксировать routing decision и inputs, но нельзя имитировать его выполнение или выдавать текст за реализацию. Реализационный запрос получает `BLOCKED`, а execution status — `NOT_RUN`, до появления разрешённого writer.

## Intake

До записи обязательны:

- один objective и один mode;
- literal allowed paths для tests и отдельно для test-support;
- исключённые paths, включая production-код;
- write-authority владельца задачи;
- accepted test-cases, requirement/risk/TC или честное отсутствие ссылки;
- acceptance, negative sentinel и stop conditions;
- execute/egress authority, рассматриваемые отдельно от write.

## Context

В `existing` нужна принятая карта нативной архитектуры и команд; новый код следует подтверждённым patterns и не создаёт параллельный test-root. В `greenfield` нужны утверждённый blueprint, выбранный пользователем stack и границы первой vertical slice.

Используйте product contract, фактический interface, representative tests и только нужный support с provenance. Непроверенная команда не становится способом запуска; секреты и реальные персональные данные не включаются.

## Prompt

Writer получает точные paths, запрещённые области, выбранный слой, cases/oracles, данные, требуемый сохранённый результат, формат changed paths/evidence и условия остановки. Prompt прямо запрещает production edits, speculative abstractions, invented commands и unobserved `PASS`.

## Design

Перед кодом подтвердите минимальное изменение:

- каждый test наблюдает один понятный сценарий;
- применимые happy/negative/authn/authz/boundary/edge/corner/failure-recovery cases выбраны из принятого design;
- данные детерминированы, isolation/cleanup определены;
- assertion проверяет существенный результат;
- fixture/client/page/helper имеет доказанный reuse или нативный precedent;
- test и support остаются в literal scope.

## Write

Когда соответствующий writer реально доступен и authority выдана, он обязан:

1. создать или изменить фактические test-файлы;
2. создать или изменить только явно разрешённый минимальный test-support;
3. не менять production-код;
4. сохранить traceability в принятой проектом форме либо честно указать её отсутствие;
5. вернуть точный перечень changed paths;
6. при обнаружении product defect остановить product-fix и передать evidence владельцу продукта.

Если сохранить код невозможно, не заменяйте его советом: верните `BLOCKED` с точной недостающей authority/context/availability.

## Evaluate

Проверьте сохранённые tests/support по [test-code-quality.md](../standards/test-code-quality.md): isolation, data determinism, meaningful assertions, local readability, bounded reuse и scenario coverage. Негативный sentinel, корректный RED или controlled mutation оценивает чувствительность assertion, но не доказывает всю систему.

## Verify

Сначала определите применимый scope проверки. Используйте только нативные команды с provenance и отдельной execute-authority. Если команда, authority или execution отсутствуют, статус соответствующей проверки — `NOT_RUN`; поведение остаётся `UNKNOWN`. Статический diff не даёт execution-`PASS`.

Независимый verifier получает raw task, changed artifacts, acceptance и evidence без авторского вывода. Writer не верифицирует собственную работу conclusion-blind.

## Handoff

Передайте changed paths, layer, traceability, выбранные cases, evidence чувствительности, фактически выполненные проверки, `NOT_RUN`/`UNKNOWN`, остаточные риски и одно следующее действие. Review и verification начинаются отдельными контрактами, не автоматически.

## Stop conditions

- Writer skill ещё недоступен или write-authority не выдана.
- Карта `existing` или blueprint `greenfield` отсутствует.
- Product oracle/interface противоречив или неизвестен.
- Изменение требует production-кода, неразрешённого support, секрета, egress или опасного security-действия.
- Единственный путь к заявлению успеха — придумать команду, output или `PASS`.

Используйте `BLOCKED`/`UNKNOWN`; всё неисполненное остаётся `NOT_RUN`.
