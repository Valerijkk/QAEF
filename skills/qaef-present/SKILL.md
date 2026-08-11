---
name: qaef-present
description: Использовать, когда нужно подготовить презентационный промпт или руководство по демонстрации QAEF
---

# Презентация продукта QAEF

## Назначение

Подготовить или адаптировать русскоязычный presentation prompt либо руководство по демонстрации QAEF, сохраняя доказательность каждого утверждения.

## Входы

Получить цель аудитории, утверждённый public manifest, literal public sources из него, желаемый формат и один output path. Для каждого факта нужны source locator и тип утверждения: fact, inference, proposal или unknown.

## Режим

Работать в указанном `existing` или `greenfield` только как с описанием пользовательского сценария. Не менять режим целевого проекта и не использовать внутренние, donor или source-project материалы как источник презентации.

## Полномочия

Читать только literal public QAEF sources и записывать только назначенный presentation prompt или demo guidance. Не открывать private evidence, не выполнять команды, не создавать слайды и не заявлять внешние результаты.

## Порядок работы

1. Проверить, что каждый источник входит в public manifest и доступен буквально.
2. Составить source registry с locator и ролью источника.
3. Пометить каждое утверждение как fact, inference, proposal или unknown.
4. Убрать либо перевести в proposal неподтверждённые ROI, клиентов, безопасность, readiness, версию или результат внедрения.
5. Подготовить prompt для 10–12 слайдов на русском языке с требуемыми визуальными deliverables.
6. Включить source inventory, speaker notes и требование render inspection без заявления, что оно уже выполнено.

## Результат

Сохранённый prompt или guidance содержит public manifest, source registry, русскоязычную аудиторию и тон, маркировку evidence, перечень deliverables и честные unknowns. Он запрашивает PPTX, PDF, PNG каждого слайда, contact sheet, notes, source inventory и render inspection, но не выдаёт их за созданные.

## Stop conditions

Вернуть `BLOCKED`, если claim не имеет literal public source, источник не входит в public manifest, язык/аудитория не определены или запрошенный результат требует внутреннего evidence. Не заменять недостающий факт оценкой.

## Evidence

Каждый fact содержит public locator. Inference ссылается на supporting facts; proposal и unknown маркируются явно. Без публичного evidence нет claim о ROI, клиентах, security, version или readiness.

## Ошибки применения

Не использовать donor/internal evidence; не смешивать факт с inference; не обещать готовность по наличию файлов; не подменять prompt изготовленными слайдами; не скрывать unsupported claim в speaker notes.

## Handoff

Передать одному владельцу сохранённый prompt, public manifest, source registry, status и одно следующее действие: проверить source traceability либо сгенерировать deliverables только при выданном отдельном полномочии.
