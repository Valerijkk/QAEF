# Жизненный цикл промпта

## Результат

Получить проверяемый prompt-contract для одной задачи: с одним режимом, ограниченным scope, входами с provenance, фактически выданной authority, наблюдаемым результатом, evidence и stop conditions. Промпт не заменяет контекст и не создаёт полномочий.

Планируемый маршрут: `$qaef-init`, если не зафиксирован контракт задачи, или `$qaef-context`, если задача определена, но её контекст недостаточен. Эти названия не подтверждают текущую доступность skills.

## Intake

Зафиксируйте по [task-contract.md](../templates/task-contract.md):

- один objective;
- ровно `existing` или `greenfield`;
- allowed/excluded paths;
- фактические read/write/execute/egress permissions;
- входы, constraints, acceptance и unknowns.

Если неоднозначность меняет режим, границу записи, источник истины или acceptance, результат — `BLOCKED` до решения владельца. Не расширяйте задачу ради более полного ответа.

## Context

Используйте task-local [context packet](../templates/context-packet.md). Каждый источник должен поддерживать конкретное решение и иметь provenance/freshness. Факты, выводы и `UNKNOWN` не смешиваются. Устаревший или конфликтующий источник не выбирается молча; секреты и ненужные персональные данные исключаются.

Для `existing` prompt не допускается к записи без принятой карты нативной архитектуры и команд. Для `greenfield` нужны решение о стеке, требования первой вертикали и утверждённый blueprint либо явная задача сначала создать его.

## Prompt

Заполните [prompt-spec.md](../templates/prompt-spec.md):

1. наблюдаемый результат;
2. режим и ответственную роль;
3. literal allowed/excluded paths;
4. входы и их происхождение;
5. разрешённые изменения и явные запреты;
6. схему выхода: changed paths, traceability, evidence, statuses, risks, next action;
7. требуемое evidence;
8. негативный пример ложного успеха или небезопасного действия;
9. stop condition для `BLOCKED`, `UNKNOWN` и `NOT_RUN`.

Если пользователь запросил реализацию, результат формулируется как реально сохранённый test/support-код. Advice, checklist и несохранённый snippet прямо исключаются. Production-код не включается в write-scope тестового writer.

Этапы `Design` и `Write` не выполняются в этом workflow: он создаёт контракт для последующего [test-design.md](test-design.md) или [write-tests.md](write-tests.md). Сохранение самого prompt-артефакта допускается только в отдельно разрешённом literal path; иначе он передаётся без скрытой записи.

## Evaluate

Проверьте prompt по [prompt-quality.md](../standards/prompt-quality.md):

- результат можно наблюдать;
- режим один, scope конечен, исключения явны;
- inputs не перепутаны с authority;
- отсутствующие сведения помечены;
- evidence соразмерно claim;
- негативный sentinel ловит advice-only, silent write, invented command или unobserved `PASS`;
- execution-ожидание остаётся `NOT_RUN`, пока не получено реальное evidence.

## Verify

Другой read-only reviewer при необходимости получает objective, источники, prompt и rubric без вывода автора. Он проверяет исполнимость и безопасность, а не переписывает prompt. Если независимость обязательна, но недоступна, статус — `BLOCKED`.

## Handoff

Передайте prompt, его режим, inputs, границы, authority, acceptance, unknowns и один следующий workflow. Handoff не запускает следующий шаг автоматически.

## Stop conditions

- Не выбран режим или отсутствует владелец конфликтующего решения.
- Требуемые paths, write-authority или источник product oracle не заданы.
- Контекст пытается расширить полномочия.
- Для завершения пришлось бы придумать команду, поведение, стек или результат.

Честный исход — `BLOCKED`/`UNKNOWN`; неисполненная проверка — `NOT_RUN`.
