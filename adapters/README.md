# Адаптеры QAEF

Адаптеры дают только installation и invocation mapping для общего ядра QAEF. Они не переводят, не копируют и не меняют contracts, agents, skills, templates, workflows или standards.

| Платформа и project instruction | Agent placement | Skill invocation | Handoff |
|---|---|---|---|
| Claude Code: `CLAUDE-QAEF.md` | Пользователь выбирает target path | `$qaef-*` из core | `templates/handoff.md` |
| Codex: `AGENTS-QAEF.md` | Пользователь выбирает target path | `skills/qaef-*/SKILL.md` + `agents/openai.yaml` | `templates/handoff.md` |
| Cursor: `QAEF-RULES.md` | Пользователь выбирает target path | `$qaef-*` из core | `templates/handoff.md` |
| JetBrains: `QAEF-PROMPTS.md` | Пользователь выбирает target path | `$qaef-*` из core | `templates/handoff.md` |

Общие инварианты: до работы выбрать ровно `existing` или `greenfield`; не расширять authority; сохранять реальные тестовые артефакты только при явном write-authority; отделять facts, inferences и `UNKNOWN`; передавать один следующий шаг одному владельцу.
