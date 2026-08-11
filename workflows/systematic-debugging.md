# Систематическая отладка тестового сбоя

## Результат

Воспроизвести один scoped failure, проверить явные гипотезы наблюдениями, установить один evidence-backed root cause и выполнить только разрешённое минимальное действие. Без воспроизведения и hypothesis evidence root cause не заявляется.

Планируемый маршрут: `$qaef-debug-tests`. Название не подтверждает текущую доступность skill.

## Intake

До debugging нужны:

- triage record с одной категорией `product|test|environment|unknown`;
- один failure instance, mode, revision и scope;
- product oracle, raw failure evidence и acceptance;
- literal read/write paths;
- отдельные read/write/execute/egress permissions;
- stop conditions и владелец результата.

Если triage отсутствует, этот workflow не классифицирует сбой задним числом: завершите текущий запуск с `BLOCKED` и запросите у владельца готовый triage record. Обратный workflow не запускается автоматически.

## Context

Соберите только относящиеся к failure sources: requirement, test, минимальный production interface для понимания границы, test-support, data/setup/cleanup, environment facts, recent changes и raw output. Каждый source имеет provenance/freshness. Секреты и лишние журналы исключаются.

Нативный способ воспроизведения берётся только из целевого проекта и сохраняется literal вместе с источником. Workflow не придумывает команду и не меняет environment ради удобства.

## Prompt

Debug-prompt задаёт одну причину для исследования, запрет на преждевременный fix, формат hypothesis ledger, допустимые probes, write-scope, критерии воспроизведения и прекращения. Product-код исключён из write-scope QAEF.

## Design: воспроизведение и гипотезы

1. **Reproduce.** Используйте только отдельно разрешённый нативный способ и зафиксируйте exact scope, revision, входные данные и raw result. Если воспроизведение не разрешено или невозможно, статус — `NOT_RUN`/`BLOCKED`, root cause — `UNKNOWN`, запись запрещена.
2. **Freeze the symptom.** Сформулируйте наблюдаемый expected/actual без объяснения причины.
3. **Hypothesis ledger.** Для каждой гипотезы запишите: предполагаемую причину, опорные facts, предсказанное наблюдение, минимальный probe, raw result и `supported|rejected|unknown`.
4. **Discriminate.** Выберите probe, который различает минимум две правдоподобные гипотезы; повтор исходного запуска без нового наблюдения не считается проверкой причины.
5. **Root cause.** Примите одну причину только когда evidence объясняет symptom и исключает существенные альтернативы. Второй независимый дефект передаётся отдельно.

Изменение expectation, skip, retry, ослабление assertion или увеличение ожидания не является диагнозом.

## Write

- При `product` production-код не меняется: подготовьте handoff его владельцу.
- При `environment` передайте минимальное evidence владельцу среды; не меняйте test как маскировку.
- При `unknown` изменений нет.
- При подтверждённом `test` и отдельной write-authority сохраните минимальный fix только в literal tests/test-support paths. Если реализация запрошена, advice или snippet не заменяют сохранённый код.

После fix обновите changed paths и не расширяйте сессию до соседних улучшений.

## Evaluate

Проверьте, что fix устраняет root cause, а не symptom; product oracle не переписан под текущий output; test сохраняет значимый assertion; новая abstraction имеет потребителя; scope не расширен. Сохраните negative sentinel для класса ошибки, если он относится к acceptance.

## Verify

До проверки заранее определите: исходное воспроизведение, изменённый test, затронутую domain regression и shared границы. Запускайте только разрешённые нативные проверки и сохраняйте raw results. Незапущенная часть остаётся `NOT_RUN`; без required execution нельзя заявлять `PASS` fix.

Conclusion-blind verifier получает task, root-cause evidence, changed artifacts и raw outputs без вывода debugger. Debugger не верифицирует собственный fix независимо.

## Handoff

Передайте исходный triage, reproduction evidence, hypothesis ledger, root cause или `UNKNOWN`, changed paths, проверки, ограничения, риски и одно следующее действие. Handoff завершает workflow; re-triage или новый fix не запускаются автоматически.

## Stop conditions

- Failure не воспроизводится либо нет разрешённого способа воспроизведения.
- Product oracle конфликтует или revision не привязана.
- Probe требует неразрешённого действия, секрета, egress или опасной среды.
- Evidence меняет исходную category и текущая authority больше не соответствует владельцу.
- Fix потребует production-кода или выхода за literal scope.

Используйте `BLOCKED`/`UNKNOWN`; отсутствующее выполнение — `NOT_RUN`.
