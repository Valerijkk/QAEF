# Review тестового кода

## Результат

Независимо и read-only проверить scoped изменение tests/test-support, вернуть evidence-linked findings, статусы проверок и verdict. Reviewer не исправляет предмет review и не расширяет scope.

Планируемый маршрут: `$qaef-review-tests`. Название не подтверждает текущую доступность skill.

## Intake

До review зафиксируйте:

- objective, один mode и acceptance;
- literal changed paths и разрешённый review-scope;
- requirement/risk/TC либо явное отсутствие traceability;
- роль автора и отдельную роль reviewer;
- разрешения на read и, отдельно, на execution;
- критерии severity и stop conditions.

Если reviewer является автором, обязательная независимость отсутствует: результат — `BLOCKED` до назначения другого reviewer. Write-authority в этом workflow всегда отсутствует.

## Context

Reviewer получает task contract, карту `existing` или blueprint `greenfield`, raw changed artifacts, product sources, accepted cases, relevant project patterns и raw evidence. Авторский отчёт считается входными данными, а не доказательством; секреты, credentials и лишние логи исключаются.

Команды проверки допустимы только если они нативны для проекта, имеют provenance и отдельно разрешены. Найденная строка без запуска остаётся `NOT_RUN`; reviewer не изобретает замену.

## Prompt

Review-request по [review-request.md](../templates/review-request.md) задаёт точные вопросы, разрешённое чтение, запрет записей, rubric, формат findings и ограничения. Он не просит «починить по ходу» и не разрешает production changes.

Этапы `Design` и `Write` не применяются: reviewer оценивает существующий предмет и остаётся read-only.

## Evaluate

Проверьте в заранее определённом scope:

1. changed paths совпадают с authority и не содержат production-код;
2. `existing` сохраняет нативную архитектуру, `greenfield` не выходит за утверждённую первую вертикаль;
3. каждый test соответствует requirement/risk/TC или честно сообщает отсутствие связи;
4. assertions наблюдают существенное поведение и чувствительны к выбранному negative sentinel;
5. данные, isolation и cleanup детерминированы;
6. reuse не скрывает сценарий и имеет подтверждённых потребителей;
7. применимые scenario classes рассмотрены;
8. claims не сильнее raw evidence, а отсутствующее выполнение — `NOT_RUN`;
9. данные и authority соблюдены.

Каждая finding содержит ID, `Blocker|Critical|Major|Minor`, точное место, нарушенное требование, expected state, наблюдаемое evidence, влияние, `CONFIRMED|PLAUSIBLE` и минимальное исправление. `PLAUSIBLE` не выдаётся за подтверждённый Blocker/Critical.

## Verify

Reviewer формирует собственные статусы каждой проверки: `PASS`, `FAIL`, `UNKNOWN`, `NOT_RUN` или `BLOCKED`. `PASS` требует reviewer-observation и точной evidence reference. Если execution является acceptance, но не проводился, общий verdict не может маскировать его как успешный.

Отдельный conclusion-blind gate при необходимости выполняется по [verification.md](verification.md); code review не присваивает ему результат заранее.

## Handoff

Передайте findings, независимые checks, verdict, limitations и одно следующее действие владельцу изменения. При findings workflow завершается handoff автору; исправление начинается новым контрактом и не запускается автоматически.

## Stop conditions

- Нет task/acceptance, raw artifacts или источника product truth.
- Reviewer не независим при обязательной независимости.
- Нужен неразрешённый доступ, execution, egress или секрет.
- Scope нельзя определить до просмотра результата.
- Авторский `PASS` предлагается принять без raw evidence.

Используйте `BLOCKED`/`UNKNOWN`; отсутствующие проверки остаются `NOT_RUN`.
