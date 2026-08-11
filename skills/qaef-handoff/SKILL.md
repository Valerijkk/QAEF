---
name: qaef-handoff
description: Использовать, когда тестовую работу нужно передать человеку или другому агенту без потери контекста
---

# Передача тестовой работы

## Назначение

Подготовить один самодостаточный handoff для следующего владельца: что фактически сделано, что не проверено и какое ровно одно действие требуется дальше.

## Входы

Получить objective и result, выбранный mode `existing` или `greenfield`, literal `changed_paths`, read-only inputs с provenance, выполненные команды с наблюдаемыми результатами, evidence, status, limitations/risks, одного получателя и одно next action. Недостающий обязательный вход не угадывать.

## Режим

Сохранить переданный режим без переключения. Handoff не создаёт новый scope, не меняет решение `existing|greenfield` и не превращает `UNKNOWN` в вывод.

## Полномочия

Работать только с выданными read-only inputs и явно назначенным handoff output. Не изменять target, тесты, SUT, evidence или чужие отчёты; не выполнять команды, не передавать данные вовне и не расширять scope.

## Порядок работы

1. Сверить literal paths и входы с выданным scope.
2. Отделить наблюдаемый result от предположений.
3. Переписать только фактически выполненные команды и их результаты; при отсутствии указать `executed_commands: []` и `execution_status: NOT_RUN`.
4. Связать каждый claim со своим evidence locator либо пометить его `UNKNOWN`.
5. Заполнить один handoff по `templates/handoff.md` и полям `standards/contracts/handoff.schema.json`.
6. Оставить ровно одно next action и одного владельца; не придумывать проверку, срок или результат.

## Результат

Сохранённый handoff содержит objective, result, mode, exact changed paths, read-only inputs, executed commands/results, evidence, status, limitations/risks, одного recipient и одно next action. Для schema-полей обязательны `result`, `changed_paths`, `evidence`, `status`, `risks` и `next_action`.

## Stop conditions

Остановиться с `BLOCKED`, если отсутствуют literal output path, mode, следующий владелец или обязательный факт нельзя безопасно отличить от догадки. При неполном evidence сохранить `NOT_RUN` или `UNKNOWN`, а не `PASS`.

## Evidence

Каждый non-`UNKNOWN` claim получает kind, locator и наблюдаемый result. `PASS` допустим только при evidence для обязательных acceptance-критериев; отсутствие выполненных проверок фиксируется как `NOT_RUN`, а не как готовность.

## Ошибки применения

Не писать «готово» без exact paths и evidence; не скрывать невыполненные проверки; не заменять path glob-ом; не выдавать предложение за результат; не добавлять второго next action или владельца.

## Handoff

Передать получателю сам handoff и ничего дополнительно не предполагать. Получатель сначала сверяет changed paths, status, risks и единственное next action с указанными evidence locators.
