# Навигация по workflows

Этот документ направляет к нужному workflow и skill, но не заменяет их тела. Все перечисленные workflow-файлы существуют как статические процессные документы. Skills опубликованы в `skills/qaef-*/SKILL.md`; наличие определения не означает, что вызов, исполнение или результат уже проверены. Перед любым маршрутом выбран ровно один режим: `existing` или `greenfield`.

| Намерение пользователя | Workflow | Skill |
|---|---|---|
| Понять существующий фреймворк до правки | `workflows/onboard-existing-framework.md` | `$qaef-map-framework` |
| Начать новый фреймворк с первой вертикали | `workflows/bootstrap-new-framework.md` | `$qaef-init` |
| Собрать или сузить контекст задачи | `workflows/context-refresh.md` | `$qaef-context` |
| Уточнить контракт промпта | `workflows/prompt-lifecycle.md` | `$qaef-init` и `$qaef-context` |
| Спроектировать покрытие требования или риска | `workflows/test-design.md` | `$qaef-design-cases` |
| Сохранить тестовый код в разрешённой области | `workflows/write-tests.md` | семь `$qaef-write-*` |
| Проверить изменения тестов | `workflows/test-code-review.md` | `$qaef-review-tests` |
| Разобрать падение | `workflows/failure-triage.md` | `$qaef-triage` |
| Исследовать причину сбоя | `workflows/systematic-debugging.md` | `$qaef-debug-tests` |
| Сверить evidence перед передачей | `workflows/verification.md` | `$qaef-gate` |
| Передать результат следующему владельцу | `workflows/handoff.md` | `$qaef-handoff` |

Для `existing` любой маршрут записи следует только после доказанной карты фреймворка. Для `greenfield` — только после зафиксированных требований первой вертикали, пользовательского выбора стека, scope и write-authority. Workflow можно применить и как процессный контракт без вызова skill; имитировать вызов нельзя: невыполненная команда остаётся `NOT_RUN`, а недостающее решение или authority — `BLOCKED`. Если другие предусловия отсутствуют, правильный результат — `UNKNOWN` или `BLOCKED`, а не переход к следующему workflow.

Полный перечень команд — в [COMMANDS.md](../COMMANDS.md); правила промптов, контекста и тестов — в [PROMPT-ENGINEERING.md](PROMPT-ENGINEERING.md), [CONTEXT-ENGINEERING.md](CONTEXT-ENGINEERING.md) и [TEST-AUTOMATION.md](TEST-AUTOMATION.md).
