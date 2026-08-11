# Cursor: подключение QAEF

Поместите `QAEF-RULES.md` в выбранный пользователем generic target installation directory `.cursor`. Этот путь является только примером размещения адаптера и не описывает topology какого-либо source project.

Rules template ссылается на core relative paths и вызывает `$qaef-*` без копирования contracts. Передайте task contract с mode, literal scope, authority, inputs, acceptance, stop conditions и одним handoff recipient.
