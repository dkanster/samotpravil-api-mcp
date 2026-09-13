# Contributing to samotpravil-mcp

Спасибо за интерес к проекту. Репозиторий временно под [dkanster/samotpravil-api-mcp](https://github.com/dkanster/samotpravil-api-mcp); после создания org Samotpravil переедет — см. [docs/ORG_MIGRATION.md](./docs/ORG_MIGRATION.md).

## Быстрый старт

```bash
git clone https://github.com/dkanster/samotpravil-api-mcp.git
cd samotpravil-api-mcp
npm install
npm test
npm run dev
```

Требования: **Node.js ≥ 18**.

Архитектура и связь Postman ↔ snapshot ↔ MCP-серверы: **[docs/ECOSYSTEM.md](./docs/ECOSYSTEM.md)**.

## Структура проекта

```
.mcp.json / mcp.json    # Open Plugins — cursor.directory auto-detect
plugin.json             # Agent Plugins manifest
src/
  index.ts              # MCP server entry, tools registration
  docs.ts               # Postman collection loader, search, format
  client.ts             # HTTP client to api.samotpravil.ru
  safety.ts             # READ_ONLY, ALLOW_SEND, ALLOW_MUTATIONS, ALLOW_GENERIC_API, dry_run
  redact.ts             # маскировка api_key и секретов в ответах
  resources.ts          # MCP Resources content
  registerResources.ts  # Resource registration
  registerAutoTools.ts  # Auto tools from snapshot
  registerPostmanTools.ts # Postman maintainer tools (POSTMAN_API_KEY)
  registerPrompts.ts    # MCP prompts
  http.ts               # Streamable HTTP transport (--http)
  postman/              # Postman API client, diff, snapshot
  tools/
    docs.ts             # Documentation tools
    api.ts              # api_request
    typed.ts            # Typed API tools
    postman.ts          # postman_* tool handlers
    sdkTyped.ts         # Python SDK parity tools
smithery.yaml           # Smithery.ai install wizard
data/
  collection.snapshot.json
  snapshot.meta.json
scripts/
  sync-docs.mjs         # Update snapshot from documenter
  check*.mjs            # Offline tests
```

## Что можно улучшить

| Тип | Куда смотреть |
|-----|----------------|
| Новый typed tool | `src/tools/typed.ts`, `src/index.ts` |
| Новый resource | `src/resources.ts`, `src/registerResources.ts` |
| Парсинг документации | `src/docs.ts` |
| Safety / env | `src/safety.ts` |
| Примеры для пользователей | `docs/EXAMPLES.md` |

Перед PR откройте issue или укажите фазу из [docs/ROADMAP_v1.4.md](./docs/ROADMAP_v1.4.md).

## Добавить typed tool

1. Добавьте Zod-схему и handler в `src/tools/typed.ts`
2. Зарегистрируйте tool в `src/index.ts`
3. Если endpoint отправляет письма — путь должен попасть в `SEND_PATH_FRAGMENTS` в `src/safety.ts`
4. Обновите README и `docs/EXAMPLES.md` при необходимости
5. `npm test`

## Обновить snapshot документации

Когда изменилась коллекция на documentation.samotpravil.ru:

```bash
npm run sync-docs
git add data/collection.snapshot.json data/snapshot.meta.json
```

В PR укажите `publishDate` и число эндпоинтов из вывода `sync-docs`.

## Тесты

```bash
npm test
```

Включает: shell syntax, TypeScript build, ESLint, docs helpers, safety, MCP resources (offline на snapshot), unit tests.

Опционально: `npm run setup-hooks` — pre-commit с `lint` + `test`.

Дополнительно:

```bash
npm run generate-tools-manifest   # data/tools.manifest.json из live MCP
npm run probe-endpoints           # probe read-only API (нужен SAMOTPRAVIL_API_KEY)
npm run plan-org-migration        # dry-run замен для org migration
npm run generate-tool-catalog   # data/tool-catalog.json
npm run scaffold-typed-tool <name>  # scaffold typed tool handler
npm run release-prepare           # pre-flight перед npm tag
```

## Стиль кода

- TypeScript strict, ESM (`"type": "module"`)
- Zod для схем tools
- Минимальный diff — не рефакторить несвязанное
- Комментарии только для неочевидной логики
- Сообщения об ошибках на русском (как в API tools)

## Коммиты и версии

[Keep a Changelog](https://keepachangelog.com/) + [SemVer](https://semver.org/):

- **patch** — bugfix, docs
- **minor** — новые tools/resources без breaking changes
- **major** — breaking changes в схемах tools или env

Обновляйте `CHANGELOG.md` и `package.json` version в PR, если меняется публичный API пакета.

## Релиз (maintainers)

См. [docs/PUBLISH.md](./docs/PUBLISH.md):

1. `NPM_TOKEN` в GitHub Secrets
2. Tag `v*`
3. Workflow `publish.yml` публикует в npm

Правки после публикации в [cursor.directory](https://cursor.directory) — там же, секция «Cursor Directory»: не создавать вторую карточку на этот репозиторий.

## Issue templates

- **Bug** — воспроизведение, окружение
- **Feature** — фаза roadmap
- **New endpoint** — если в Postman появился новый метод

## Вопросы

- API СамОтправил: https://documentation.samotpravil.ru/
- Доступ к SMTP/API: https://samotpravil.ru/get-access
- Issues: https://github.com/dkanster/samotpravil-api-mcp/issues
