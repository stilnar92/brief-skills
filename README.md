# brief-skills

Скиллы Claude Code для того, чтобы все роли прорабатывали задачу вместе с AI до того, как написан код. Цель — убрать поздние возвраты по цепочке Product → Design → Backend → Frontend → QA.

## Главный принцип

Бриф пишется на языке пользователя: что делаем, зачем, флоу пользователя и edge cases. Технические детали (хранение данных, SDK, как отследить событие) в бриф не попадают: их роли решают в своих задачах.

## Скиллы

| Скилл | Кто | Что делает |
|---|---|---|
| `/brief-product` | Product owner | `draft`: AI задаёт вопросы по шаблону, затем создаёт Jira-эпик и задачу «Бриф». `publish`: после созвона переводит бриф в Ready и создаёт задачи реализации по ролям |
| `/brief-review` | Backend → Frontend/iOS/Android → Design → QA | Ревью брифа глазами роли с учётом ревью предыдущих ролей. Разработчики отвечают на вопросы по коду. Результат — задача «Ревью <роль>» в эпике |
| `/brief-meeting` | Общий созвон | Сводит бриф и все ревью: убирает дубли, первыми выносит противоречия, по ходу встречи вносит решения в бриф |
| `/brief-dev` | Разработчики | Реализация в рамках брифа, отклонения — только через Changelog |
| `/brief-qa` | QA | Тест-кейсы из брифа, баги со ссылкой на раздел, пробелы в брифе отдельно от багов |

Общая карта процесса и правила: [skills/brief-product/ecosystem.md](skills/brief-product/ecosystem.md). Шаблон брифа: [skills/brief-product/template.md](skills/brief-product/template.md).

## Установка (Claude Code)

```
/plugin marketplace add stilnar92/brief-skills
/plugin install brief-skills@brief-skills
```

## Настройка

Каждой роли нужен подключённый **Atlassian MCP** (Jira + Confluence): через него скиллы читают бриф и ревью других ролей. Подробности — в разделе «Настройка» файла [skills/brief-product/SKILL.md](skills/brief-product/SKILL.md).
