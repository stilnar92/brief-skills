# brief-skills

Скиллы Claude Code для того, чтобы все роли прорабатывали задачу вместе с AI до того, как написан код. Цель — убрать поздние возвраты по цепочке Product → Design → Backend → Frontend → QA.

## Главный принцип

Бриф пишется на языке пользователя: что делаем, зачем, флоу пользователя и edge cases. Технические детали (хранение данных, SDK, как отследить событие) в бриф не попадают: их роли решают в своих задачах.

## Где заканчивается процесс

На публикации: у всех есть итоговый бриф и своя задача в Jira-эпике. Как реализовывать и проверять, каждая роль решает сама.

## Скиллы

| Скилл | Кто | Что делает |
|---|---|---|
| `/brief-product` | Product owner | `draft`: AI задаёт вопросы по шаблону, затем создаёт Jira-эпик и задачу «Бриф». `publish`: после созвона переводит бриф в Ready и создаёт задачи всем ролям |
| `/brief-review` | Backend → Frontend/iOS/Android → Design → QA | Ревью брифа глазами роли с учётом ревью предыдущих ролей. Разработчики отвечают на вопросы по коду. Результат — задача «Ревью <роль>» в эпике |
| `/brief-meeting` | Общий созвон | Сводит бриф и все ревью: убирает дубли, первыми выносит противоречия, по ходу встречи вносит решения в бриф |

Общая карта процесса и правила: [skills/brief-product/ecosystem.md](skills/brief-product/ecosystem.md). Шаблон брифа: [skills/brief-product/template.md](skills/brief-product/template.md).

## Установка (Claude Code)

### Вариант 1. Промт — скопируй и вставь в Claude Code

```
Установи скиллы brief-* из репозитория https://github.com/stilnar92/brief-skills:
1. Склонируй репозиторий во временную папку.
2. Скопируй папки skills/brief-product, skills/brief-review и skills/brief-meeting
   в ~/.claude/skills/ (если такие папки уже есть — покажи разницу и спроси, перезаписать ли).
3. Удали временную папку.
4. Проверь, что в ~/.claude/skills/ есть brief-product/SKILL.md, brief-product/ecosystem.md,
   brief-product/template.md, brief-review/SKILL.md и brief-meeting/SKILL.md.
5. Проверь, подключён ли у меня Atlassian MCP (команда `claude mcp list`). Если нет — спроси
   разрешения и выполни: claude mcp add --transport http -s user atlassian https://mcp.atlassian.com/v2/mcp
   Затем скажи мне ввести /mcp → atlassian и войти в Atlassian через браузер.
6. В конце перечисли команды /brief-product, /brief-review, /brief-meeting и одной строкой —
   что делает каждая. Скиллы станут доступны в новой сессии Claude Code.
```

Обновление — тот же промт: скиллы перезапишутся последней версией.

### Вариант 2. Как плагин (вручную)

```
/plugin marketplace add stilnar92/brief-skills
/plugin install brief-skills@brief-skills
```

Эти команды вводятся в Claude Code руками: Claude не может запустить `/plugin` сам. Используй один из двух вариантов, иначе скиллы будут в двух экземплярах.

## Обязательно: подключить Atlassian MCP

Без него скиллы не видят Jira: не прочитают бриф и ревью других ролей и не создадут задачи. Подключает **каждый**, кто пользуется скиллами (продукт, дизайн, бекенд, фронт, мобилка, QA), один раз.

**Claude Code**
1. В терминале:
   ```
   claude mcp add --transport http -s user atlassian https://mcp.atlassian.com/v2/mcp
   ```
2. В Claude Code: `/mcp` → `atlassian` → войти через браузер под своим аккаунтом Atlassian.
3. Проверка: попросить «найди задачу DPP-<номер> в Jira».

**claude.ai (веб / десктоп):** Settings → Connectors → Atlassian → Connect.

AI видит и меняет только то, что доступно тебе самому в Jira и Confluence. Без MCP скиллы работают в ручном режиме: бриф и ревью вставляются текстом, результат заводится в Jira руками.

Product owner дополнительно указывает проект Jira (и, если нужно, пространство Confluence) — раздел «Куда публиковать» в [skills/brief-product/SKILL.md](skills/brief-product/SKILL.md).
