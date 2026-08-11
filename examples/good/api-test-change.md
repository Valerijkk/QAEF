# Принятый пример: изменение API-теста

## Запрос

> Режим `existing`. Добавить API-тесты чтения заказа: владелец получает ресурс, другой пользователь — отказ по правам, а запрос без учётных данных — отказ по идентификации. Production-код не менять.

## Выбранный контекст

| Источник | Наблюдаемый факт | Решение, которое он поддерживает |
|---|---|---|
| `specs/orders.md` | Владелец получает `200`, другой пользователь `403`, запрос без учётных данных `401`; отказ не раскрывает сумму заказа. | Oracle для authn, authz и control-сценария. |
| `tests/api/test_order_read_access.py` | API-тесты вызывают `api_client.get(f"/orders/{order.id}", ...)`, проверяют `status_code`; denial-ответы читаются через `response.json()`. | Существующий слой, endpoint, способ вызова и чтения ответа. |
| `tests/support/orders.py` | `order_factory(*, owner_id: str, amount: str)` создаёт синтетический заказ и возвращает объект с `id`. | Подтверждённый интерфейс подготовки существующего ресурса. |
| `tests/support/identity.py` | `auth_headers(user_id: str) -> dict[str, str]` создаёт заголовки для указанного синтетического пользователя. | Подтверждённый интерфейс идентификации. |

**Факты:** ресурс должен существовать; fixtures и ожидаемые ответы подтверждены проектными источниками.

**Вывод:** контроль владельца и отказ другому пользователю нужно наблюдать в одном сценарии, чтобы `403` не оказался ложноположительным из-за недоступного всем ресурса. Отдельный сценарий покрывает отсутствие идентификации.

**Неизвестное:** error codes и остальные поля denial-body не заданы; способ исполнения не предоставлен.

## Сохранённый путь

- `tests/api/test_order_read_access.py`

Сохранённые тесты:

```python
def test_stranger_cannot_read_an_existing_order(
    api_client,
    order_factory,
    auth_headers,
) -> None:
    order = order_factory(owner_id="owner-a", amount="1500.50")

    owner_response = api_client.get(
        f"/orders/{order.id}",
        headers=auth_headers("owner-a"),
    )
    stranger_response = api_client.get(
        f"/orders/{order.id}",
        headers=auth_headers("user-b"),
    )

    assert owner_response.status_code == 200
    assert stranger_response.status_code == 403
    stranger_body = stranger_response.json()
    assert "amount" not in stranger_body


def test_anonymous_request_cannot_read_order(api_client, order_factory) -> None:
    order = order_factory(owner_id="owner-a", amount="1500.50")

    response = api_client.get(f"/orders/{order.id}")

    assert response.status_code == 401
    body = response.json()
    assert "amount" not in body
```

## Evidence

- Статическое evidence: два сохранённых сценария находятся в существующем API-файле и используют подтверждённый test-support.
- Traceability: `specs/orders.md` → owner `200`, stranger `403`, anonymous `401` и отсутствие `amount` в denial-body; другие body-поля не закрепляются.
- Синтетические идентификаторы и сумма не содержат реальных пользовательских данных.
- Evidence исполнения и очистки fixture отсутствует.

## Честный статус

- Артефакт подготовлен: да; показаны сохраняемый путь и содержимое.
- Статус выполнения: `NOT_RUN`.
- Поведение целевой системы: `UNKNOWN` до наблюдаемой проверки.
- Остаточный риск: error codes, остальные body-поля, фактическая изоляция и очистка `order_factory` не подтверждены.
