# Codex: подключение QAEF

Codex находит каждый skill QAEF по `skills/qaef-*/SKILL.md` и соответствующему `agents/openai.yaml`. Подключите инструкции проекта через `AGENTS-QAEF.md` в выбранном пользователем целевом пути и предоставьте те же публичные agents и workflows, которые определены в основном наборе QAEF.

Не дублируйте skill bodies, не добавляйте plugin или MCP dependency и не меняйте семантику contracts. Для каждого вызова передайте mode, literal scope, authority, inputs, acceptance, stop conditions и одного handoff recipient.
