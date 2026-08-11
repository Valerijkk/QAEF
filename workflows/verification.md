# Независимая верификация evidence

## Результат

Conclusion-blind, read-only и независимо решить, поддерживает ли raw evidence конкретный claim в заранее определённом scope. Verifier проверяет достижение acceptance, а не факт выполнения task-list, и не исправляет предмет проверки.

Планируемый маршрут: `$qaef-gate`. Название не подтверждает текущую доступность skill.

## Intake

До просмотра результата зафиксируйте:

- один claim, один mode и acceptance;
- verifier, отличный от автора;
- literal subject paths и forbidden writes;
- полный требуемый scope: новое поведение, применимая regression, traceability, architecture, data/security;
- разрешённые read/execute действия;
- критерии статусов и stop conditions.

Если verifier не независим или scope предлагается выбрать после просмотра удобного evidence, результат — `BLOCKED`.

## Context без авторского вывода

Verifier получает:

- task contract и acceptance;
- source requirements/risks/TC;
- карту `existing` или blueprint `greenfield`;
- raw changed artifacts или diff;
- raw outputs и locators фактически выполненных checks;
- provenance нативных команд и revision;
- ограничения и отсутствующие материалы.

Verifier **не получает вывод, verdict, ожидаемый score или убеждающее summary автора** до собственного заключения. Имена каталогов, ярлыки и приложенный `PASS` не считаются evidence. Если отделить авторский вывод от raw artifacts невозможно, зафиксируйте limitation или `BLOCKED`.

## Prompt

Verification-prompt содержит claim, acceptance, frozen scope, read-only authority, raw inputs, rubric, требуемый формат [verification-report.md](../templates/verification-report.md) и stop conditions. Запись, self-repair и внешняя передача запрещены.

Этапы `Design` и `Write` не применяются.

## Evaluate

Verifier независимо проверяет:

1. фактические changed paths и соблюдение authority;
2. соответствие `existing`-карте или `greenfield`-blueprint;
3. traceability requirement/risk/TC -> test;
4. существенность assertions, данные, isolation и bounded reuse;
5. negative sentinel/RED/controlled mutation, если это acceptance;
6. raw evidence нового поведения;
7. заранее определённую domain/adjacent regression, если она применима;
8. отсутствие секретов, лишних персональных данных и неразрешённых действий.

Команды запускаются только при отдельной execute-authority и только если они доказаны как нативные. Иначе соответствующий check — `NOT_RUN`, а не inferred `PASS`.

## Verify: собственный verdict

Для каждого check укажите `PASS|FAIL|UNKNOWN|NOT_RUN|BLOCKED` и evidence refs. Итоговый статус не может быть сильнее самой слабой обязательной acceptance:

- `PASS` — все обязательные checks имеют наблюдаемое evidence в frozen scope;
- `FAIL` — evidence противоречит обязательному критерию;
- `UNKNOWN` — данных недостаточно для вывода;
- `NOT_RUN` — обязательное execution evidence не получено;
- `BLOCKED` — нет независимости, authority, context или решения владельца.

Статический `PASS` допустим только для явно статического claim и не означает выполнение, готовность, отсутствие дефектов или безопасность вне scope.

После фиксации собственного verdict verifier может сравнить его с выводом автора. Расхождение сохраняется как evidence conflict; оно не решается голосованием и не стирается.

## Handoff

Передайте claim, frozen scope, evidence, checks, независимый status, limitations, conflicts, risks и одно следующее действие. Verifier не исправляет subject; rework начинается новым контрактом у владельца.

## Stop conditions

- Автор и verifier совпадают при обязательной независимости.
- Нет raw artifacts, acceptance, revision или привязки evidence.
- Проверка требует неразрешённой команды, egress, секрета или записи.
- Предоставлен только author verdict без воспроизводимого evidence.
- Required scope нельзя определить до результата.

Используйте `BLOCKED`/`UNKNOWN`; всё неисполненное остаётся `NOT_RUN`.
