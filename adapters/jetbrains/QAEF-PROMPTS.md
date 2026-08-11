# Промпты QAEF

Используйте общие output contracts QAEF без изменения их семантики.

| Запрос | Skill | Требуемый результат |
|---|---|---|
| Карта существующего фреймворка | `$qaef-map-framework` | Сохранённая карта и `NOT_RUN` для неисполненных команд. |
| Новый фреймворк | `$qaef-init` | Явный `greenfield` contract и следующий разрешённый шаг. |
| Анализ покрытия | `$qaef-coverage` | Риск-ранжированные gaps и один writer handoff. |
| Передача работы | `$qaef-handoff` | Exact paths, evidence, status, risks и один next action. |
| Материал для руководителя | `$qaef-present` | Русскоязычный evidence-labelled prompt из public sources. |

До вызова выберите `existing` или `greenfield`, задайте literal scope и authority. Возвращайте facts, inferences и `UNKNOWN` отдельно; не утверждайте `PASS` без evidence.
