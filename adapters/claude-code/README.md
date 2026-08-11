# Claude Code: подключение QAEF

Поместите `CLAUDE-QAEF.md` в выбранный пользователем project-instruction path. Разместите определения ролей там, где их discovery поддерживается конкретной установкой; этот adapter не назначает обязательный filesystem layout.

Подключайте skills из общего `skills/qaef-*/SKILL.md` по их точным именам. До реализации передайте полный task contract с mode, scope, authority, inputs, acceptance и одним handoff recipient. Core contracts не копируйте и не изменяйте в adapter.
