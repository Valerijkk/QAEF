# Проектирование тестов

## Результат

Преобразовать подтверждённое требование или риск в минимальный трассируемый test-plan и набор test-cases с наблюдаемыми oracles, данными, слоями и automation paths. Этот workflow проектирует, но не реализует тестовый код.

Планируемый маршрут: `$qaef-design-cases`. Название не подтверждает текущую доступность skill.

## Intake

Зафиксируйте:

- один objective и один режим;
- requirement, риск или TC либо явное отсутствие ссылки;
- acceptance, in/out scope и ограничения;
- read/write authority для design-артефактов отдельно от будущего test-кода;
- желаемый набор сохраняемых design paths;
- stop conditions.

Если product oracle отсутствует или противоречив, не проектируйте уверенное ожидание: статус — `BLOCKED`/`UNKNOWN`.

## Context

Для `existing` обязательна принятая карта архитектуры и нативных команд; будущие tests/support должны оставаться в ней. Для `greenfield` обязательны требования первой вертикали, выбранный пользователем стек и утверждённый blueprint.

Добавьте только релевантные product contracts, interfaces, representative tests, data rules, risks и known failures с provenance. Не используйте чужой пример как authority и не переносите его layout без решения проекта.

## Prompt

Prompt задаёт проектировщику один набор поведения, выбранный mode, sources, границы design, запрещённую запись test/production-кода, формат plan/cases, rubric и stop conditions. Если пользователь одновременно запросил реализацию, prompt должен разделить design-артефакт и последующую запись через [write-tests.md](write-tests.md); plan не считается кодом.

## Design

Заполните [test-plan.md](../templates/test-plan.md) и нужные [test-case.md](../templates/test-case.md):

1. сопоставьте requirement/risk/TC с наблюдаемой границей;
2. выберите один или несколько уровней только по риску: unit, contract, API, integration, UI, load или security;
3. рассмотрите happy, negative, authn, authz, boundary, edge, corner и failure-recovery;
4. для каждого класса обоснуйте включение или исключение;
5. задайте детерминированные данные, предусловия, изоляцию и очистку;
6. сформулируйте существенный oracle;
7. выберите RED, controlled mutation или negative sentinel для чувствительности assertion, либо отметьте `NOT_RUN`;
8. добавляйте fixture/client/page/helper только при подтверждённом reuse или нативном соглашении;
9. укажите literal automation path либо честное отсутствие пути.

Фиксированная тестовая пирамида, обязательное дерево каталогов и speculative support запрещены.

## Write

При отдельной write-authority сохраните только согласованные design-артефакты. Реальные tests/support здесь не создаются. Если реализация запрошена, handoff обязан маршрутизировать approved subset в [write-tests.md](write-tests.md); advice-only итог не закрывает задачу реализации.

## Evaluate

Оцените design по [test-architecture.md](../standards/test-architecture.md), [test-code-quality.md](../standards/test-code-quality.md) и rubric из [EVALUATION.md](../docs/EVALUATION.md):

- каждый case связан с риском и наблюдаемой границей;
- oracle способен отличить нужное поведение от ложного успеха;
- scenario matrix рассмотрена полностью, но не раздута неприменимыми cases;
- данные и support минимальны;
- спорные cases остаются открытыми, не маскируются как принятые.

Разрешена частичная передача принятого подмножества, если открытые cases явно помечены и не влияют на него.

## Verify

Независимый read-only reviewer получает requirements, raw plan/cases и rubric без вывода автора. Он проверяет traceability, oracles, границы и негативные sentinels; он не правит design. Выполнение тестов на этой стадии `NOT_RUN`.

## Handoff

Передайте принятые design paths, матрицу traceability, открытые вопросы, planned writer layer, статусы и одно следующее действие. Новый контракт на реализацию выбирает ровно один маршрут из [write-tests.md](write-tests.md).

## Stop conditions

- Нет product truth, риска или достаточной границы наблюдения.
- Режимная карта/blueprint отсутствует.
- Нужны неразрешённые данные или security-действие.
- Требуется изобрести стек, команду, interface или expected behavior.

Результат — `BLOCKED`/`UNKNOWN`; execution status — `NOT_RUN`.
