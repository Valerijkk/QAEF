---
name: qaef-write-unit-tests
description: Использовать, когда нужно создать или изменить изолированные unit-тесты в стеке целевого проекта
---

# Написание unit-тестов

## Назначение

Сохранять изолированные unit-тесты для принятого поведения в literal target-пути. Результат задачи — сохранённый diff разрешённых test-файлов, а не совет или кодовый snippet в ответе.

## Входы

Нужны task contract с режимом и полномочиями, требования и риски, принятые TC, карта фреймворка и literal target-файлы. Использовать только переданные REQ/RISK/TC, данные и Oracle; новые требования, риски, TC и пути не проектировать.

## Режим

До работы выбрать ровно один режим: `existing` или `greenfield`.

- В `existing` сохранить доказанные target-команды, слой, стиль, layout и паттерн representative test.
- В `greenfield` использовать только одобренный blueprint и literal target-пути из контракта.
- Не менять режим без явного решения пользователя.

## Полномочия

Читать только contract-входы и target-файлы в его read scope. Записывать только literal test-пути и явно разрешённый test-support из write allowlist, применяя правку к реальному файлу.

Не изменять production-код, конфигурацию, зависимости или неразрешённые support-файлы. Не расширять authority, не изобретать команду и не выполнять egress. Если `execute=false`, не запускать ничего; defect production-поведения передать владельцу продукта отдельным handoff.

## Порядок работы

1. Подтвердить режим, read/write authority, принятые REQ/RISK/TC, native pattern и target path.
2. Для каждого недостающего TC назвать production break; проверить применимость классов `happy`, `negative`, `authn`, `authz`, `boundary`, `edge`, `corner` и `failure-recovery`; выбрать только обоснованные требованиями и рисками классы, один наблюдаемый behavior и независимые literal data и Oracle.
3. Написать test в native style: direct call реального SUT, точный REQ/RISK/TC trace, один behavior на `test_*`, literal expected value или literal exception Oracle.
4. Сохранить правку в exact target через разрешённый механизм. Обязательная форма результата: сохранённый target diff, затем его manifest и статусы. Snippet может пояснять diff, но никогда не заменяет сохранённый файл.
5. Если contract разрешает изменение implementation, сначала получить test-first failure evidence через разрешённый native запуск. Этот skill не меняет production. Когда production read-only, не фабриковать RED и не менять SUT.
6. Зафиксировать evidence и handoff; при недоступном запуске оставить `NOT_RUN`, а не `PASS`.

## Результат

Вернуть exact changed paths, краткий diff-summary, REQ/RISK/TC trace, data/Oracle, production break и assertion-sensitivity. Отдельно указать `executed_commands`, execution, mutation/RED и runtime status. Без сохранённого разрешённого test diff задача не выполнена.

## Stop conditions

Остановиться с `BLOCKED`, если нет выбранного режима, literal write authority, принятого поведения/TC, native target pattern или target path. Зафиксировать `UNKNOWN`, если необходимый факт нельзя подтвердить. Не заменять stop выдуманной командой, production-правкой или заявлением `PASS`.

## Evidence

Проверить и передать семь инвариантов writer:

1. Явное production-поведение — требования и literal Oracle.
2. Нативный паттерн цели — конфигурация, representative test и карта фреймворка.
3. Mutation-aware evidence — назван production break; выполненная проверка либо честный `NOT_RUN`.
4. Реальное поведение — прямой вызов SUT и assertion результата или исключения.
5. Независимое ожидание — hand-derived literals без логики SUT или её helpers.
6. Минимальные doubles — mock/spy/stub только при доказанной необходимости; для чистой функции их нет.
7. Реальный сохранённый путь — diff существует только в разрешённом target test/test-support.

## Ошибки применения

Не выдавать только advice/snippet, не дублировать уже покрытый TC и не смешивать behaviors. Не проверять отсутствие ошибки, source text или mock вместо продукта; не вычислять expected через SUT. Не заявлять запуск, RED, sensitivity или `PASS`, если это не наблюдалось.

## Handoff

Передать получателю режим, exact changed paths, сохранённый diff-summary, trace, data/Oracle, production break, evidence, `executed_commands`, все статусы, неизвестности/риски и одно следующее действие. Не расширять authority и не завышать readiness.
