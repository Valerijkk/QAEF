# Принятый пример: минимальная greenfield-вертикаль

## Запрос

> Режим `greenfield`. Пользователь выбрал Python и pytest. Создать первую unit-вертикаль для расчёта цены после скидки. Разрешена запись только в `tests/unit/`; production-код и другие каталоги исключены.

## Выбранный контекст

| Источник | Наблюдаемый факт | Решение, которое он поддерживает |
|---|---|---|
| Решение пользователя в контракте задачи | Выбраны Python, pytest, `greenfield` и граница `tests/unit/`. | Стек, режим и write-scope. |
| `specs/pricing.md` | Скидка `10` процентов от `1000` даёт итог `900`; скидка `0` сохраняет исходную сумму. | Happy- и boundary-oracle. |
| `src/pricing/discount.py` | Модуль `pricing.discount` экспортирует публичную функцию `discounted_total(amount, percent)`. | Фактический import и точка наблюдения без изменения production-кода. |

**Факты:** у проекта нет согласованного тестового layout; первая вертикаль и стек выбраны пользователем.

**Вывод:** для первой вертикали достаточно одного файла с двумя representative tests. Общий helper пока не нужен: подтверждённого повторного использования нет.

**Неизвестное:** способ исполнения и требования к общей конфигурации не предоставлены; они не придумываются.

## Сохранённый путь

- `tests/unit/test_discounted_total.py`

Сохранённый тест:

```python
from pricing.discount import discounted_total


def test_discounted_total_applies_ten_percent_discount() -> None:
    assert discounted_total(amount=1000, percent=10) == 900


def test_discounted_total_keeps_amount_for_zero_percent() -> None:
    assert discounted_total(amount=1000, percent=0) == 1000
```

## Evidence

- Статическое evidence: первая вертикаль представлена реальным сохраняемым test-файлом, а не схемой каталогов или советом.
- Traceability: `specs/pricing.md` → happy `10%` и boundary `0%` → два локально читаемых assertions.
- Вне первой вертикали не добавлены fixtures, клиенты или общие слои без потребителя.
- Evidence исполнения, RED или controlled mutation отсутствует.

## Честный статус

- Артефакт подготовлен: да; показаны сохраняемый путь и содержимое.
- Статус выполнения: `NOT_RUN`.
- Поведение целевой системы: `UNKNOWN` до наблюдаемой проверки.
- Остаточный риск: импорт и фактическая семантика функции не подтверждены исполнением.
