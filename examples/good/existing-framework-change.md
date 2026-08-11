# Принятый пример: изменение в существующем фреймворке

## Запрос

> Режим `existing`. Добавить негативный API-тест для количества товара `0`. Разрешено изменить только существующий тест и его подтверждённый helper. Production-код не менять.

## Выбранный контекст

| Источник | Наблюдаемый факт | Решение, которое он поддерживает |
|---|---|---|
| `specs/cart.md` | Допустимое целое количество — от `1` до `99`; значение `0` отклоняется с кодом ошибки `invalid_quantity`. | Oracle и boundary-сценарий. |
| `tests/api/test_cart_items.py` | Файл импортирует `cart_item_payload` из `tests.support.cart_payloads`; API-тесты вызывают `api_client.post("/cart/items", json=...)` и читают доменный код через `response.json()["error"]["code"]`. | Существующий import, слой, endpoint и способ чтения oracle. |
| `tests/support/cart_payloads.py` | Общий helper имеет сигнатуру `cart_item_payload(*, sku: str = "SKU-001") -> dict[str, object]` и возвращает `{"sku": sku, "quantity": 1}`. | Подтверждённая точка минимального изменения test-support. |

**Факты:** режим, oracle, тестовый корень и helper подтверждены указанными источниками.

**Вывод:** минимальное изменение — расширить существующий helper параметром и добавить один boundary-тест в текущий файл.

**Неизвестное:** HTTP status и остальные поля ответа спецификацией не заданы; подтверждённый способ исполнения не предоставлен.

## Сохранённые пути

- `tests/support/cart_payloads.py`
- `tests/api/test_cart_items.py`

Сохранённое изменение test-support:

```python
def cart_item_payload(*, sku: str = "SKU-001", quantity: int = 1) -> dict[str, object]:
    return {"sku": sku, "quantity": quantity}
```

Сохранённый тест:

```python
from tests.support.cart_payloads import cart_item_payload


def test_add_item_rejects_zero_quantity(api_client) -> None:
    response = api_client.post(
        "/cart/items",
        json=cart_item_payload(quantity=0),
    )

    body = response.json()
    assert body["error"]["code"] == "invalid_quantity"
```

## Evidence

- Статическое evidence: два перечисленных пути содержат показанное изменение.
- Traceability: `specs/cart.md` → boundary `quantity = 0` → `test_add_item_rejects_zero_quantity`.
- Assertion наблюдает ровно подтверждённый доменный код ошибки; HTTP status и прочая форма ответа не закрепляются.
- Evidence исполнения и чувствительности assertion отсутствует.

## Честный статус

- Артефакт подготовлен: да; показаны два сохраняемых пути и содержимое.
- Статус выполнения: `NOT_RUN`.
- Поведение целевой системы: `UNKNOWN` до наблюдаемой проверки.
- Остаточный риск: HTTP status, остальные поля ответа и фактическое соответствие доменному oracle не проверены.
