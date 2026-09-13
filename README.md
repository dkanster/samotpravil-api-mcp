# Samotpravil MCP

[![CI](https://github.com/dkanster/samotpravil-api-mcp/actions/workflows/ci.yml/badge.svg)](https://github.com/dkanster/samotpravil-api-mcp/actions/workflows/ci.yml)
[![npm](https://img.shields.io/npm/v/samotpravil-mcp.svg)](https://www.npmjs.com/package/samotpravil-mcp)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

MCP-сервер вокруг [документации API СамОтправил](https://documentation.samotpravil.ru/) и HTTP API `api.samotpravil.ru`.

**Версия:** 1.8.0 · **npm:** [`samotpravil-mcp`](https://www.npmjs.com/package/samotpravil-mcp) · **MCP Registry:** `io.github.dkanster/samotpravil-api-mcp` · **Smithery:** [`smithery.yaml`](./smithery.yaml) · **Cursor Directory:** [`.mcp.json`](./.mcp.json)

> **Хостинг:** репозиторий временно в [dkanster/samotpravil-api-mcp](https://github.com/dkanster/samotpravil-api-mcp).  
> **Планируется:** переезд в org **Samotpravil** → `@samotpravil/mcp` — [docs/ORG_MIGRATION.md](./docs/ORG_MIGRATION.md).

Сервер подтягивает Postman-коллекцию с documenter (live + offline snapshot) и даёт агенту tools для поиска методов, вызова API и безопасных пресетов (`READ_ONLY`, `dry_run`). Имена typed tools совпадают с [Python SDK `samotpravil`](https://pypi.org/project/samotpravil/).

**Экосистема:** Postman → snapshot → MCP / OpenAPI / Docusaurus — [docs/ECOSYSTEM.md](./docs/ECOSYSTEM.md) · live preview: **https://dkanster.github.io/samotpravil-api-mcp/**

---

## Быстрый старт

```bash
npx -y samotpravil-mcp@latest
```

**Cursor** — `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "samotpravil": {
      "command": "npx",
      "args": ["-y", "samotpravil-mcp@latest"],
      "env": {
        "SAMOTPRAVIL_API_KEY": "your_api_key_here",
        "SAMOTPRAVIL_READ_ONLY": "1",
        "SAMOTPRAVIL_ALLOW_SEND": "0"
      }
    }
  }
}
```

`SAMOTPRAVIL_API_KEY` опционален для docs-only tools. После правок: **Settings → MCP → Reload**.

Бесплатного тестового ключа нет: регистрация в ЛК → верификация домена отправителя → ключ в env. Пакетную боевую отправку по умолчанию не открываем (`SAMOTPRAVIL_ALLOW_SEND=0`).

Сценарии и конфиги для Claude / VS Code: **[docs/EXAMPLES.md](./docs/EXAMPLES.md)**

### Cursor Directory

Каталог сообщества [cursor.directory](https://cursor.directory) автодетектит MCP из корневого [`.mcp.json`](./.mcp.json) (дубль: [`mcp.json`](./mcp.json), манифест [`plugin.json`](./plugin.json)). Отправка: [cursor.directory/plugins/new](https://cursor.directory/plugins/new) → URL этого репозитория. В листинг не входят swagger-mcp и Postman maintainer.

---

## Что внутри

| Компонент | Кол-во | Нужен ключ |
|-----------|--------|------------|
| Docs tools | 4 | нет |
| Core typed API | 9 + `api_request` | `SAMOTPRAVIL_API_KEY` |
| Python SDK parity | 28 | `SAMOTPRAVIL_API_KEY` |
| Auto tools (`api_*`) | ~16 | `SAMOTPRAVIL_API_KEY` |
| Postman maintainer | 4 | `POSTMAN_API_KEY` |
| MCP Resources | 9 | нет |
| MCP Prompts | 5 | нет |

**Итого:** ~59 tools ( +4 postman при `POSTMAN_API_KEY`).

---

## Инструменты

### Документация (без API-ключа)

| Tool | Описание |
|------|----------|
| `get_overview` | Авторизация, SMTP, лимиты, категории |
| `list_endpoints` | Список всех методов API |
| `search_docs` | Поиск по документации |
| `get_endpoint` | Подробности по методу |

### Typed API (нужен `SAMOTPRAVIL_API_KEY`)

| Tool | Описание |
|------|----------|
| `send_email` | POST `/api/v1/smtp_send` |
| `send_mail_v2` | POST `/api/v2/mail/send` |
| `get_delivery_status` | GET `/api/v2/issue/status` (`message_id` или `x_track_id`) |
| `get_package_status` | GET `/api/v2/package/status` |
| `search_stop_list` | Поиск email в стоп-листах |
| `add_stop_list_email` / `remove_stop_list_email` | Стоп-лист (`mail_from` или `domain`) |
| `validate_email` | POST `/api/v2/emails/validate/` |
| `list_allowed_domains` | GET `/api/v2/blist/domains` |
| `api_request` | Generic escape hatch |

### Python SDK parity (v1.3+)

Typed tools с именами как в PyPI-пакете `samotpravil`: `send_package`, `get_statistics`, `get_ext_status`, `stop_list_export_create`, `domain_add`, `get_blist`, `create_authkey` и др.

Полный список и маппинг: **[docs/EXAMPLES.md#python-sdk-parity](./docs/EXAMPLES.md#python-sdk-parity)** · prompt `python_sdk_parity`

### Postman maintainer (нужен `POSTMAN_API_KEY`)

| Tool | Описание |
|------|----------|
| `postman_get_collection` | Коллекция из Postman API |
| `postman_sync_snapshot` | Postman API → `data/collection.snapshot.json` |
| `postman_diff_snapshot` | Diff Postman vs локальный snapshot |
| `postman_search_requests` | Поиск запросов в коллекции |

Подробнее: **[docs/EXAMPLES.md#postman-tools](./docs/EXAMPLES.md#postman-tools)**

### Auto tools

`api_{method}_{path}` — для HTTP-методов, не покрытых typed tools (legacy v1, tickets, email check/clean и т.д.).

### MCP Prompts

| Prompt | Описание |
|--------|----------|
| `integration_overview` | Обзор SMTP + HTTP + лимиты |
| `send_transactional` | Чеклист отправки письма |
| `stop_list_workflow` | Работа со стоп-листами |
| `check_delivery` | Статус по X-Track-ID / выпуску |
| `python_sdk_parity` | Python SDK → MCP tools |

### MCP Resources

| URI | Содержимое |
|-----|------------|
| `samotpravil://overview` | Обзор API |
| `samotpravil://endpoints` | Индекс методов |
| `samotpravil://endpoint/{slug}` | Один метод |
| `samotpravil://errors` | Популярные ошибки |
| `samotpravil://integration` | SMTP, X-Track-ID, трекинг |
| `samotpravil://sdk-mapping` | Python SDK → MCP tools |
| `samotpravil://changelog` | Фрагмент CHANGELOG пакета |
| `samotpravil://rate-limits` | Лимиты API и отправки |
| `samotpravil://api-wishlist` | Предложения по HTTP API (фрагмент) |

---

## Безопасность

| Env | Эффект |
|-----|--------|
| `SAMOTPRAVIL_READ_ONLY=1` | Только GET/HEAD |
| `SAMOTPRAVIL_ALLOW_SEND=0` | Блок send/package |
| `SAMOTPRAVIL_ALLOW_MUTATIONS=0` | Блок stop-list, доменов, authkey |
| `SAMOTPRAVIL_ALLOW_GENERIC_API=0` | Отключить `api_request` |
| `SAMOTPRAVIL_DOCS_MODE` | `auto` \| `live` \| `snapshot` |
| `dry_run: true` | Preview запроса без отправки |

Секреты (`api_key`, `key=` в query) маскируются в ответах MCP.

---

## Транспорты и интеграции

### HTTP transport

```bash
npx samotpravil-mcp --http --port 3000
# POST http://127.0.0.1:3000/mcp
```

Env: `SAMOTPRAVIL_HTTP_HOST`, `SAMOTPRAVIL_HTTP_PORT`, `SAMOTPRAVIL_HTTP_AUTH_TOKEN`, `SAMOTPRAVIL_HTTP_JSON_LOG=1` (structured logs).

```bash
docker build -t samotpravil-mcp .
docker run --rm -p 3000:3000 -e SAMOTPRAVIL_API_KEY=... -e SAMOTPRAVIL_HTTP_AUTH_TOKEN=... samotpravil-mcp
```

### OpenAPI + Swagger-MCP

```bash
npm run export-openapi        # → data/openapi.yaml
npm run upload-swaggerhub     # SwaggerHub (нужен .env.swaggerhub)
npm run prepare-swagger-mcp   # Vizioz/Swagger-MCP
```

Спека: [mailganer/samotpravil-smtp-api@1.0.0](https://app.swaggerhub.com/apis/mailganer/samotpravil-smtp-api/1.0.0) · [docs/SWAGGERHUB.md](./docs/SWAGGERHUB.md)

### Docusaurus preview

```bash
npm run docusaurus:install && npm run docusaurus:start
```

Live: **https://dkanster.github.io/samotpravil-api-mcp/** · [docs/DOCS_SITE.md](./docs/DOCS_SITE.md)

### Discovery

| Площадка | Ссылка |
|----------|--------|
| npm | https://www.npmjs.com/package/samotpravil-mcp |
| MCP Registry | https://registry.modelcontextprotocol.io |
| Smithery | `smithery.yaml` в корне — [docs/PUBLISH.md](./docs/PUBLISH.md) |
| Официальный promo | [docs/official/](./docs/official/) |

---

## Конфигурация

Шаблон: [`.env.samotpravil.example`](./.env.samotpravil.example)

```env
SAMOTPRAVIL_API_KEY=your_key_here
# POSTMAN_API_KEY=...          # maintainer tools
# SAMOTPRAVIL_READ_ONLY=1
```

Ключ API: https://samotpravil.ru/get-access

Полный список env: **[docs/EXAMPLES.md](./docs/EXAMPLES.md#переменные-окружения)**

---

## Разработка

```bash
git clone https://github.com/dkanster/samotpravil-api-mcp.git
cd samotpravil-api-mcp
npm install && npm test && npm run dev
npm run setup-hooks   # optional: pre-commit (lint + test)
npm run lint          # ESLint
npm run pre-publish-check   # перед npm tag
npm run release-prepare      # pre-flight перед npm tag
npm run generate-tool-catalog
npm run scaffold-typed-tool send_package
```

- Security: **[SECURITY.md](./SECURITY.md)**
- Contributing: **[CONTRIBUTING.md](./CONTRIBUTING.md)**
- Changelog: **[CHANGELOG.md](./CHANGELOG.md)**
- Publish: **[docs/PUBLISH.md](./docs/PUBLISH.md)**
- Roadmap: **[docs/ROADMAP_v1.6.md](./docs/ROADMAP_v1.6.md)** · Release: **[docs/RELEASE_v1.7.0.md](./docs/RELEASE_v1.7.0.md)**
- Org migration: **[docs/ORG_MIGRATION_RUNBOOK.md](./docs/ORG_MIGRATION_RUNBOOK.md)** · `npm run plan-org-migration`
- API v1→v2: **[docs/MIGRATION_V1_TO_V2.md](./docs/MIGRATION_V1_TO_V2.md)**
- **API wishlist** (предложения для HTTP API продукта): **[docs/API_WISHLIST.md](./docs/API_WISHLIST.md)**

### Источник документации

- Live: https://documentation.samotpravil.ru/
- Offline: `data/collection.snapshot.json`
- Обновить: `npm run sync-docs` или `postman_sync_snapshot`
- API: https://api.samotpravil.ru · SMTP: `api.samotpravil.ru:1126` / `:1127`

### Из git clone в свой проект

```bash
/path/to/samotpravil-api-mcp/setup.sh .
```

---

## Лицензия

MIT
