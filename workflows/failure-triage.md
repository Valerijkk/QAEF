# Классификация падения

## Результат

До любого изменения отнести один наблюдаемый сбой ровно к одной категории: `product`, `test`, `environment` или `unknown`, зафиксировать evidence и безопасного следующего владельца. Классификация не является исправлением и не равна `PASS`.

Планируемый маршрут: `$qaef-triage`. Название не подтверждает текущую доступность skill.

## Intake

Нужны один failure instance, один mode, исходный scope, revision/freshness либо `UNKNOWN`, raw symptom, ожидаемое поведение и authority. Несвязанные падения разделяются на отдельные triage records.

До verdict запрещены изменения tests, expectations, production-кода, данных и environment. Retry, skip, ослабление assertion или изменение timeout не используются для сокрытия неизвестной причины.

## Context

Соберите минимальный пакет:

- product requirement/oracle с provenance;
- точный test и относящийся test-support;
- raw failure output и указатель на источник;
- изменения, способные повлиять на сбой;
- факты environment/config/data, но не секреты;
- нативный способ воспроизведения с provenance, если он известен;
- предыдущие наблюдения только при сопоставимой revision.

Команда, которую не нашли в проектном источнике и не согласовали, остаётся `UNKNOWN`. Отсутствующий запуск — `NOT_RUN`.

## Prompt

Triage-prompt фиксирует запрет на изменения, четыре допустимые категории, требуемое evidence, критерии выбора, формат handoff и stop conditions. Он запрещает подгонять expectation под текущее product-поведение до проверки product truth.

Этапы `Design` и `Write` не применяются.

## Evaluate: четыре категории

| Категория | Минимальное основание | Следующее действие |
|---|---|---|
| `product` | Надёжный product oracle противоречит наблюдаемому поведению, а test проверяет именно этот oracle. | Read-only handoff владельцу production-кода; QAEF не исправляет product. |
| `test` | Product oracle и доступное наблюдение согласуются, но test, данные, setup, isolation или assertion им противоречат. | Новый ограниченный debug/fix-контракт для tests/support. |
| `environment` | Evidence связывает сбой с конфигурацией, доступностью зависимости, ресурсом, данными среды или способом исполнения, не требуя изменения oracle. | Handoff владельцу среды; не маскировать test/product изменением. |
| `unknown` | Evidence недостаточно, противоречиво, устарело или не позволяет отличить первые три категории. | Запросить конкретное evidence либо открыть bounded debugging; изменений нет. |

Нестабильность сама по себе не пятая категория. Пока evidence не связывает её с test или environment, классификация остаётся `unknown`.

## Verify

Сверьте verdict с raw facts и альтернативами: какое наблюдение исключает каждую другую категорию? Если исключить альтернативы нельзя, используйте `unknown`. При high-impact acceptance другой read-only reviewer получает raw inputs и критерии без triage-вывода автора.

## Handoff

Передайте category, confidence, facts, отвергнутые альтернативы, `NOT_RUN`, риски и одного владельца. Допустимые следующие пути:

- `product` -> владелец product defect;
- `test` -> новый запуск [systematic-debugging.md](systematic-debugging.md) или подтверждённая минимальная test-задача;
- `environment` -> владелец environment;
- `unknown` -> запрос конкретного context/evidence или bounded debugging.

Это описание next action, а не автоматический запуск или цикл.

## Stop conditions

- Нет raw symptom, product oracle или привязки к revision.
- Требуется неразрешённое выполнение, egress или доступ к секрету.
- Источники product truth конфликтуют без владельца решения.
- Единственное обоснование — предположение по тексту ошибки.

Используйте `unknown` и `UNKNOWN`; неисполненное воспроизведение — `NOT_RUN`.
