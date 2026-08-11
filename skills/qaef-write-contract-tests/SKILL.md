---
name: qaef-write-contract-tests
description: Использовать, когда нужно проверить совместимость API, schema, event или source contract в целевом проекте
---
# Написание contract-тестов

## Назначение

Сохранять contract-тест выбранной provider/consumer границы, который наблюдает совместимость, а не только валидность одной стороны.

## Входы

Нужны один режим, literal write scope, provider и named consumer contract, requirement/risk/TC либо их отсутствие, accepted cases, native map/blueprint и target pattern. Consumer expectation имеет независимое provenance.

## Режим

Выберите `existing` или `greenfield`. В `existing` сохраните доказанные layout, imports и native command. В `greenfield` нужны выбранный stack, approved blueprint и literal test path.

## Полномочия

Читайте только переданные contracts и target context. Принимайте только синтетические либо явно разрешённые минимальные contract data; не копируйте реальные secrets, tokens, PII или raw sensitive payloads в tests, evidence и handoff. Записывайте только literal test/test-support paths с write-authority; production, configuration и неразрешённый support не меняйте. Execute и egress требуют отдельных authority.

## Порядок работы

1. Назовите provider, consumer и наблюдаемую boundary; свяжите каждый test с requirement/risk/TC либо явно отметьте отсутствие связи.
2. Выведите expected contract из consumer, не из provider/SUT. Для каждого mismatch назовите observable break и mutation, которую assertion сделает красной.
3. Сохраните native test, сравнивающий обе стороны: проверяйте required fields и допустимые values consumer; добавьте hand-derived compatible control.
4. Source-level contract допускается только для реальной boundary с названной mutation. Self-validation provider, source grep, mirrored expected и assertion отсутствия ошибки не заменяют comparison.
5. Используйте native command только при `execute=true`; иначе сохраните `NOT_RUN`.

## Результат

Верните saved test/support diff, exact changed paths, provider/consumer boundary, trace, data/oracle, breaks/mutations, compatible control, `executed_commands`, execution status, limitations и risks.

## Stop conditions

Верните `BLOCKED`, если нет consumer expectation, literal scope, authority, map/blueprint или observable oracle. Верните `UNKNOWN` при конфликте contract truth. Не придумывайте command, output или `PASS`.

## Evidence

Проверьте семь инвариантов: requirement/risk/TC и map; только применимые happy/negative/authn/authz/boundary/edge/corner/failure-recovery cases; named production break; реальный saved test/support diff; RED, controlled mutation или negative sentinel; фактическая native command либо `NOT_RUN`; отсутствие production edits. Assertions должны читать обе contract стороны и независимый compatible control.

## Ошибки применения

Не валидируйте provider сам по себе, не ищите текст schema, не копируйте provider в expected и не пропускайте consumer-required field/value. Не выдавайте snippet за saved test или static diff за execution success.

## Handoff

Передайте одному reviewer/verifier exact paths, boundary, trace, break/mutation per assertion, compatible-control rationale, evidence, `NOT_RUN`/`UNKNOWN`, risks и одно next action.
