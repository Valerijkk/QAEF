---
name: qaef-triage
description: Использовать, когда нужно отделить дефект продукта, теста, данных или среды до исправления
---

# Triage тестового сбоя

## Назначение

До любого ремонта классифицировать падение как `product|test|environment|unknown` и назначить следующего владельца. Skill не подчиняется заранее выбранному диагнозу и по умолчанию не изменяет assertion, тест или продукт.

## Входы

До triage получить: source-of-truth requirement/contract; failing test и literal test scope; релевантное production behavior; reproduction/environment evidence; owners для возможных классов; exclusions. Просьба «это точно test bug» не заменяет contract.

## Режим

Работать в закрытом read-only triage scope. Не переходить к repair, не ослаблять assertion и не менять классификацию без нового evidence; repair требует отдельного task contract и writer authority.

## Полномочия

Разрешено только читать переданные raw artifacts и формировать classification/handoff. Запрещены write, execute и egress; нет права менять production, тесты, данные, окружение, retries или assertions.

## Порядок работы

1. Проверить наличие source-of-truth behavior и отделить его от предположений о намерении автора.
2. Зафиксировать failure и reproduction evidence с locators; неисполненный reproduction — `NOT_RUN`.
3. Сопоставить observed behavior с contract, test oracle и environment facts.
4. Выбрать ровно одну primary classification: `product|test|environment|unknown`; указать confidence `CONFIRMED|PLAUSIBLE` и отдельно перечислить альтернативы.
5. Назначить следующего owner: `product` для product defect, `test` для test defect, `environment` для environment defect; для `unknown` — owner следующего discriminating evidence, не владельца произвольного repair.
6. Если behavior или reproduction отсутствуют, вернуть `UNKNOWN`, объяснить точный missing input и не менять assertion для «зелёного» результата.

## Результат

Вернуть один triage record: source-of-truth, observations, reproduction status, primary classification, confidence, alternatives, owner, exact next evidence/action и ограничения. Классификация не является исправлением и не доказывает отсутствие дефектов за пределами scope.

## Stop conditions

Вернуть `BLOCKED`, если нет literal scope/owner или отсутствует требуемая authority для безопасного следующего шага. Вернуть `UNKNOWN`, если отсутствует/противоречив requirement/contract либо нет достаточного reproduction evidence. Не выбирать `test` только потому, что это требует меньшего изменения.

## Evidence

Evidence ceiling: каждому non-`UNKNOWN` выводу нужен raw-artifact locator; contract, test, production behavior и environment evidence не взаимозаменяемы. Не выполненная проверка — `NOT_RUN`; предположение — `PLAUSIBLE`, а не подтверждённая classification.

## Ошибки применения

Не принимать предвыбранный «test bug», не ослаблять assertion, не скипать contract check, не обвинять среду без фактов и не ремонтировать symptom вместо классификации. Не смешивать evidence автора с независимым triage observation.

## Handoff

Передать одному владельцу: primary classification и confidence, contract/test/behavior/environment locators, reproduction status, alternatives, `UNKNOWN`/`NOT_RUN`, ограничения и одно следующее действие — собрать названный missing evidence или выдать отдельный repair contract владельцу classification.
