# Инструкции QAEF

Используй QAEF core files как единственный источник contracts, agents, skills, templates, workflows и standards.

До работы выбери ровно `existing` или `greenfield`, зафиксируй literal scope и authority. В `existing` сначала вызови `$qaef-map-framework`; в `greenfield` — `$qaef-init`. Не меняй production-код при задаче тестов. Любой факт без locator остаётся `UNKNOWN`; невыполненная команда остаётся `NOT_RUN`.

При write-authority сохраняй реальные тесты или явно разрешённый test-support только в выданных paths. Заверши одним handoff по `templates/handoff.md`: exact changed paths, evidence, risks, status и одно next action одному владельцу.
