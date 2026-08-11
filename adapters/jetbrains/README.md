# JetBrains: подключение QAEF

Добавьте `QAEF-PROMPTS.md` в выбранную пользователем prompt library или project instructions. Конкретный filesystem convention не предполагается. Подключайте общие skills по точным `$qaef-*` именам и сохраняйте единые output contracts.

Adapter не дублирует QAEF core и не создаёт дополнительные integrations. Каждый prompt получает mode, literal scope, authority, inputs, acceptance, stop conditions и одного handoff recipient.
