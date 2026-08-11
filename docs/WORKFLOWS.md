# Навигация по workflows

Этот документ направляет к нужному workflow и skill, но не заменяет их тела. Все перечисленные workflow-файлы существуют как статические процессные документы. Skills пока планируются и недоступны: их имя не означает, что вызов или исполнение уже возможно. Перед любым маршрутом выбран ровно один режим: `existing` или `greenfield`.

| Намерение пользователя | Workflow | Skill |
|---|---|---|
| Понять существующий фреймворк до правки | `workflows/onboard-existing-framework.md` | Планируется, пока недоступен: `$qaef-map-framework` |
| Начать новый фреймворк с первой вертикали | `workflows/bootstrap-new-framework.md` | Планируется, пока недоступен: `$qaef-init` |
| Собрать или сузить контекст задачи | `workflows/context-refresh.md` | Планируется, пока недоступен: `$qaef-context` |
| Уточнить контракт промпта | `workflows/prompt-lifecycle.md` | Планируются, пока недоступны: `$qaef-init` и `$qaef-context` |
| Спроектировать покрытие требования или риска | `workflows/test-design.md` | Планируется, пока недоступен: `$qaef-design-cases` |
| Сохранить тестовый код в разрешённой области | `workflows/write-tests.md` | Планируются, пока недоступны: семь `$qaef-write-*` |
| Проверить изменения тестов | `workflows/test-code-review.md` | Планируется, пока недоступен: `$qaef-review-tests` |
| Разобрать падение | `workflows/failure-triage.md` | Планируется, пока недоступен: `$qaef-triage` |
| Исследовать причину сбоя | `workflows/systematic-debugging.md` | Планируется, пока недоступен: `$qaef-debug-tests` |
| Сверить evidence перед передачей | `workflows/verification.md` | Планируется, пока недоступен: `$qaef-gate` |
| Передать результат следующему владельцу | `workflows/handoff.md` | Планируется, пока недоступен: `$qaef-handoff` |

Для `existing` любой маршрут записи следует только после доказанной карты фреймворка. Для `greenfield` — только после зафиксированных требований первой вертикали, пользовательского выбора стека, scope и write-authority. Пока нужный skill отсутствует, workflow можно прочитать и применить как процессный контракт, но нельзя имитировать вызов: реализационный запрос остаётся `BLOCKED`, выполнение — `NOT_RUN`. Если другие предусловия отсутствуют, правильный результат — `UNKNOWN` или `BLOCKED`, а не переход к следующему workflow.

Полный перечень команд — в [COMMANDS.md](../COMMANDS.md); правила промптов, контекста и тестов — в [PROMPT-ENGINEERING.md](PROMPT-ENGINEERING.md), [CONTEXT-ENGINEERING.md](CONTEXT-ENGINEERING.md) и [TEST-AUTOMATION.md](TEST-AUTOMATION.md).
