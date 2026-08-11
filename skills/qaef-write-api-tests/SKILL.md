---
name: qaef-write-api-tests
description: Использовать, когда нужно проверить публичное HTTP/API-поведение, ошибки, авторизацию и границы данных
---
# Написание API-тестов

## Назначение

Сохранять native API-тесты публичной границы, которые проверяют документированный ответ, ошибку, disclosure и применимые side effects, а не только status code.

## Входы

Нужны один режим, objective, literal read/write scope, method/path и request/response/error contract, auth model, accepted cases, requirement/risk/TC либо их отсутствие, native map/blueprint, client/fixtures/representative test, command provenance, safe deterministic data и authority.

## Режим

Выберите `existing` или `greenfield`. В `existing` сохраните доказанные layout, imports, client, fixtures, data lifecycle и native command. В `greenfield` нужны approved blueprint, выбранный stack и минимальный vertical test path; универсальный runner не создавайте.

## Полномочия

Читайте только переданный contract и target context. Записывайте реальные tests и только отдельно разрешённый минимальный test-support в literal paths; production, configuration и dependencies не меняйте. Используйте синтетические либо явно разрешённые минимальные data; не копируйте реальные secrets, tokens, PII или raw sensitive payloads в tests, evidence и handoff. Execute и egress требуют отдельных authority.

## Порядок работы

1. Назовите endpoint/behavior и trace. Разведите happy, negative/not-found, authn, authz и boundary; edge, corner и failure-recovery включайте только при доказанной применимости, иначе объясните исключение.
2. Выведите expected status, body/schema, error, disclosure и side effects из API/product contract, не из SUT, client implementation или actual response. Не придумывайте undocumented fields, messages или codes.
3. Подготовьте deterministic existing/missing IDs, principals, isolation и cleanup. Authn и authenticated-but-unauthorized authz проверяйте раздельно; access case должен доказывать отсутствие защищённых данных.
4. Сохраните native test в разрешённом path. Каждый существенный assertion связывайте с observable production break и RED, controlled mutation либо negative sentinel.
5. Выполняйте только доказанную native command при `execute=true`; иначе сохраните `executed_commands: []`, `execution_status: NOT_RUN` и runtime behavior `UNKNOWN`.

## Результат

Верните saved test/support diff, exact changed paths, endpoint и cases, trace, data/oracle provenance, break/sensitivity per assertion, side-effect/disclosure evidence, `executed_commands`, execution/runtime status, limitations и risks.

## Stop conditions

Верните `BLOCKED`, если нет mode, route/interface, literal write authority, map/blueprint, accepted case, auth model для access decision, safe deterministic data, target pattern или observable oracle. Конфликт product truth — `UNKNOWN`. Не изобретайте command, output или `PASS`.

## Evidence

Проверьте семь инвариантов: named API behavior и trace; native client/fixture pattern; mutation-aware evidence; реальный public boundary call с meaningful body/error/disclosure/side-effect assertions; independent contract-derived expectation; только justified authorized support/doubles; реальный saved path. Static diff не является execution success.

## Ошибки применения

Не ограничивайтесь status-only или happy path, не смешивайте authn с authz, не используйте случайный missing ID, shared mutable state или production data. Не выводите expected из implementation, не добавляйте parallel client/root и не выдавайте snippet либо static inspection за выполненный runtime `PASS`.

## Handoff

Передайте одному reviewer/verifier exact paths, endpoint contract, case/trace matrix, data и oracle provenance, break/sensitivity, disclosure/side effects, evidence, честные `NOT_RUN`/`UNKNOWN`, risks и одно next action. Product defect передайте владельцу, не исправляя production.
