---
name: qaef-debug-tests
description: Использовать, когда тест падает, зависает, нестабилен или расходится между средами
---

# Диагностика тестовых сбоев

## Назначение

Доказательно диагностировать падение, зависание, flaky-поведение или межсредовое расхождение. По умолчанию skill не исправляет файлы: он возвращает проверяемую causal hypothesis или честный `UNKNOWN`.

## Входы

До диагностики получить: literal read scope и exclusions; failure/log с provenance; source/test context; requirement или наблюдаемое expected behavior; environment facts; approved command, если воспроизведение разрешено; отдельное явное authority на любой fix. Отсутствующий reproduction не заменять сообщением «CI срочно».

## Режим

Работать в диагностическом режиме и закрытом scope. Отдельный fix допустим только после классификации, explicit user authority и отдельного test-first writer contract; текущая диагностика остаётся read-only.

## Полномочия

Разрешено читать переданные artifacts и анализировать evidence. Запрещены запись, изменение timeout/retry, правка теста/продукта и egress. Выполнить можно только literal approved command, только при `authority.execute: true` и разрешённых scope/environment; иначе воспроизведение остаётся `NOT_RUN` либо `BLOCKED`. Approved command не выдаёт право на любую другую команду или на исправление.

## Порядок работы

1. Зафиксировать reproduction status: `PASS|FAIL|UNKNOWN|NOT_RUN|BLOCKED`; при отсутствии разрешённого запуска — `NOT_RUN`.
2. Разделить наблюдения с locator от hypotheses; составить как минимум одну конкурирующую причину, когда evidence её не исключает.
3. Сначала сверить source-of-truth behavior, затем классифицировать наиболее поддержанную причину как `product|test|environment|unknown` с confidence `CONFIRMED|PLAUSIBLE`.
4. Выбрать один наименьший discriminating experiment, который различает ведущую и альтернативную hypothesis; без утверждённой команды указать его как proposed, `NOT_RUN`.
5. Связать causal chain с raw evidence. Fixed delay, timeout increase или retry допустимы как remediation только при доказанной причинной связи, не как удобная реакция на давление.
6. Если нужна правка, передать точный scope, classification и experiment отдельному владельцу; самому ничего не менять.

## Результат

Вернуть один diagnostic record: observations, reproduction status, hypotheses и alternatives, classification, confidence, root-cause evidence, smallest discriminating experiment, исключённые symptom patches, limitations и один handoff. `root_cause: UNKNOWN` обязателен до достаточного evidence.

## Stop conditions

Вернуть `BLOCKED`, если нет source/test/log scope или отсутствует authority, нужное для безопасного следующего действия. Вернуть `UNKNOWN`, если expected behavior, environment facts или evidence противоречивы/недостаточны. Не назначать product, test или environment виновником по одному сообщению об ошибке.

## Evidence

Evidence ceiling: raw log, literal source locator, environment fact и результат разрешённого воспроизведения — разные виды evidence. Неисполненная команда остаётся `NOT_RUN`; hypothesis без прямого подтверждения — `PLAUSIBLE`; отсутствие причины не является PASS.

## Ошибки применения

Не повышать timeout, добавлять sleep/retry, ослаблять assertion или винить environment без causal evidence. Не выдавать one-shot diagnosis за root cause, не скрывать alternative hypothesis и не применять fix из диагностического ответа.

## Handoff

Передать одному владельцу: classification, reproduction status, observations и locators, ведущую/альтернативную hypotheses, discriminating experiment, `UNKNOWN`/`NOT_RUN`, ограничения и одно следующее действие — разрешить experiment либо выдать отдельный test-first fix contract.
