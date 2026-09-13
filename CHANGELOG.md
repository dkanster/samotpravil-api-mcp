# Changelog

All notable changes to this project are documented in this file.

Format based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.9.0](https://github.com/dkanster/samotpravil-api-mcp/compare/v1.8.0...v1.9.0) (2026-09-13)


### Features

* add Open Plugins manifests for cursor.directory ([fd40ead](https://github.com/dkanster/samotpravil-api-mcp/commit/fd40eadb5121a9f23f022bc7ad01c158cd7581dd))
* check-scaffold-ship for upstream v2 tool readiness ([#70](https://github.com/dkanster/samotpravil-api-mcp/issues/70)) ([39c6011](https://github.com/dkanster/samotpravil-api-mcp/commit/39c6011dc07c3df6ef21034a906eef8b055c7207))
* Docusaurus smoke workflow and org migration issue template ([#62](https://github.com/dkanster/samotpravil-api-mcp/issues/62)) ([04230f0](https://github.com/dkanster/samotpravil-api-mcp/commit/04230f0f623fa9a61e2830bf74bec8e1de8042c5))
* maintainer status report and promo materials v1.8.0 ([#66](https://github.com/dkanster/samotpravil-api-mcp/issues/66)) ([18a1b30](https://github.com/dkanster/samotpravil-api-mcp/commit/18a1b3051d43bf7948f6da7bbd1ccd0b93c6aaaa))
* naming decision C, org/promo handoff --write ([#71](https://github.com/dkanster/samotpravil-api-mcp/issues/71)) ([46aa895](https://github.com/dkanster/samotpravil-api-mcp/commit/46aa895639b8a87333149bf193a30f8d7ec56c08))
* org migration preflight and scaffold checks ([#61](https://github.com/dkanster/samotpravil-api-mcp/issues/61)) ([9563d67](https://github.com/dkanster/samotpravil-api-mcp/commit/9563d67d1fbe48d6f99d2362278d2fd41074b9f2))
* promo checklist automation and CI v1.9 readiness ([#73](https://github.com/dkanster/samotpravil-api-mcp/issues/73)) ([48ff931](https://github.com/dkanster/samotpravil-api-mcp/commit/48ff931dd8e11970d6ea7411585a5f5c40f4cdaa))
* promo handoff and superseded PR tracker ([#68](https://github.com/dkanster/samotpravil-api-mcp/issues/68)) ([b10619c](https://github.com/dkanster/samotpravil-api-mcp/commit/b10619c5bf022ed15698223ce3025c1ed09577ce))
* release PR body generator with readiness snapshot ([#75](https://github.com/dkanster/samotpravil-api-mcp/issues/75)) ([9575b2f](https://github.com/dkanster/samotpravil-api-mcp/commit/9575b2f55ff65df496b868204520705392a8f5ca))
* release-please check and maintainer bundle ([#74](https://github.com/dkanster/samotpravil-api-mcp/issues/74)) ([ef75899](https://github.com/dkanster/samotpravil-api-mcp/commit/ef75899002c2e58e614f494b36d9a65a17fff46a))
* repo naming decision docs and plan-rename tooling ([#67](https://github.com/dkanster/samotpravil-api-mcp/issues/67)) ([71c9e34](https://github.com/dkanster/samotpravil-api-mcp/commit/71c9e34d2a3abf498b31f9a2fb2d8d04047ebc45))
* v1.9 readiness aggregator and weekly handoff artifacts ([#72](https://github.com/dkanster/samotpravil-api-mcp/issues/72)) ([1a1efa9](https://github.com/dkanster/samotpravil-api-mcp/commit/1a1efa9ab3d8169a7dc0627f83e8cf006a5b0c58))
* weekly org preflight and Scalar reference CI checks ([#63](https://github.com/dkanster/samotpravil-api-mcp/issues/63)) ([2b1f114](https://github.com/dkanster/samotpravil-api-mcp/commit/2b1f114562441f495df6a21e167734f6a6e33bb6))


### Bug Fixes

* skip rename-alternative guard after variant B applied ([e8b33ff](https://github.com/dkanster/samotpravil-api-mcp/commit/e8b33ff774c6457836b60abf88f5348fda914306))

## [Unreleased]

### Added

- Repo naming strategy doc (`docs/REPO_NAMING.md`) and `npm run plan-rename`
- Promo handoff (`npm run promo-handoff`) and superseded PR tracker (`npm run check-superseded-prs`)
- Maintainer status weekly workflow
- `npm run check-scaffold-ship` — readiness when upstream endpoints land in snapshot
- `npm run org-handoff` — handoff comment for org migration issue #65
- Naming decision recorded: variant C in `data/org-migration.targets.json`
- `npm run check-v19-readiness` — aggregate v1.9 release criteria
- `npm run check-promo-checklist` — promo checklist automation
- `npm run release-pr-body` — release PR body with readiness snapshot
- `npm run check-python-sdk-pr` — PR #22 v2.0 track status
- [docs/ROADMAP_v2.0.md](./docs/ROADMAP_v2.0.md) Python MCP milestone

## [1.8.0] - 2026-07-13

### Added

- Upstream wishlist watch workflow — auto-issue when proposed endpoints appear in snapshot
- Phase 7 maintainer tooling: `apply-org-migration`, `maintainer-pr-cleanup.sh`, branch protection & release-please docs
- `post-release.yml` + `verify-publish` для проверки npm/MCP Registry после publish
- Snapshot-aware `scaffold-typed-tool` с `--write` → `scaffolds/*.ts.stub`
- Wishlist scaffold templates для proposed v2 endpoints
- [docs/RELEASE_v1.8.0.md](./docs/RELEASE_v1.8.0.md)

### Changed

- **zod 4.4.3** — `z.record(key, value)` two-arg form; MCP SDK ^1.29
- **ESLint 10** + typescript-eslint 8.63
- GitHub Actions: checkout v7, setup-node v6, pages deploy v5
- Docusaurus 3.10.2

### Dependencies

- Safe bumps consolidated (#52); deferred majors documented in [DEPENDENCY_DEFERRALS.md](./docs/DEPENDENCY_DEFERRALS.md)

## [1.7.0] - 2026-07-12

### Added

- `data/tool-catalog.json` — enriched catalog (group, method, path) from manifest + snapshot
- `npm run generate-tool-catalog`, `check-tool-catalog`, `check-sdk-parity`, `check-upstream-wishlist`
- `scripts/scaffold-typed-tool.mjs` — codegen scaffold для новых typed tools
- `data/upstream-wishlist.json` — tracking proposed v2 endpoints
- `npm run release-prepare` — pre-flight перед npm tag
- [docs/RELEASE_v1.7.0.md](./docs/RELEASE_v1.7.0.md)

### Changed

- `tools.manifest.json` теперь включает `group`, `method`, `path`, `groups`
- `SDK_TYPED_TOOL_COUNT`: 29 → 28 (исправление под фактическое число SDK tools)

### Fixed

- Несоответствие `SDK_TYPED_TOOL_COUNT` и реального числа зарегистрированных SDK tools

## [1.6.0] - 2026-07-12

### Added

- Org migration prep: `data/org-migration.targets.json`, `check-org-migration`, `plan-org-migration`
- [docs/ORG_MIGRATION_RUNBOOK.md](./docs/ORG_MIGRATION_RUNBOOK.md) — пошаговый runbook
- `npm run pre-publish-check` + gate в `publish.yml`
- [docs/official/PROMO_CHECKLIST.md](./docs/official/PROMO_CHECKLIST.md) + issue template `docs_promo`
- [docs/MIGRATION_V1_TO_V2.md](./docs/MIGRATION_V1_TO_V2.md) — гид для интеграторов
- [docs/ROADMAP_v1.6.md](./docs/ROADMAP_v1.6.md)
- MCP Resource `samotpravil://api-wishlist`
- `mcp.json.example.org` — конфиг после org migration
- `.github/branch-protection.example.json`

### Changed

- `RESOURCE_COUNT`: 8 → 9

## [1.5.0] - 2026-07-12

### Added

- Nightly workflow `.github/workflows/probe-endpoints.yml` (read-only API probe with `SAMOTPRAVIL_API_KEY` secret)
- `data/tools.manifest.json` codegen (`npm run generate-tools-manifest`) + CI drift check
- MCP integration test (`scripts/check-mcp-integration.mjs`) — stdio client, list tools/prompts/resources
- Structured HTTP logging (`src/httpLog.ts`, env `SAMOTPRAVIL_HTTP_JSON_LOG=1`)
- MCP Resource `samotpravil://rate-limits`
- Release Please workflow (`.github/workflows/release-please.yml`)

### Changed

- `probe-endpoints.mjs` reads `SAMOTPRAVIL_API_KEY` from env (CI-friendly)
- `RESOURCE_COUNT`: 7 → 8

## [1.4.0] - 2026-07-12

### Added

- Dependabot, `SECURITY.md`, `CODEOWNERS`
- ESLint (`npm run lint`) и unit-тесты `node:test` (safety, redact, endpointMeta)
- CI: openapi drift check, snapshot age warning, npm audit
- Scheduled workflow `.github/workflows/sync-docs.yml` (weekly snapshot PR)
- MCP Tool Annotations (`readOnlyHint`, `destructiveHint`) на всех tools
- HTTP auth: `SAMOTPRAVIL_HTTP_AUTH_TOKEN` (Bearer) для `--http` deploy
- Docker image (`Dockerfile`) для HTTP transport
- `scripts/sync-versions.mjs` — синхронизация `package.json` → `server.json`
- MCP Resources: `samotpravil://sdk-mapping`, `samotpravil://changelog`
- Pre-commit hook: `npm run setup-hooks`
- Roadmap: [docs/ROADMAP_v1.4.md](./docs/ROADMAP_v1.4.md)

### Changed

- `RESOURCE_COUNT`: 5 → 7

## [1.3.2] - 2026-07-11

### Fixed

- `server.json` description length ≤100 chars (MCP Registry validation 422)

## [1.3.1] - 2026-07-11

### Fixed

- MCP Registry CI: publish runs after npm (`workflow_run`), with retry until package is visible on npm

### Added

- `smithery.yaml` — Smithery install wizard (stdio via `npx samotpravil-mcp@latest`)
- `scripts/check-smithery.mjs` — offline validation of Smithery metadata
- `server.json`: env `SAMOTPRAVIL_HTTP_PORT`, `SAMOTPRAVIL_DOCS_MODE`

## [1.3.0] - 2026-07-11

### Added

- **29 typed tools** с именами как в Python SDK `samotpravil` (`send_package`, `get_statistics`, `stop_list_export_create`, …)
- MCP Prompt `python_sdk_parity` — маппинг Python SDK → MCP tools
- Алиас `domain` → `mail_from` в `add_stop_list_email` / `remove_stop_list_email`
- `get_delivery_status` принимает `message_id` или `x_track_id`

### Changed

- Auto tools сокращены: пути SDK parity исключены из `api_*` генерации
- Обновлены promo-материалы `docs/official/` (MCP Registry, Python SDK)

## [1.2.1] - 2026-07-11

### Added

- Встроенные Postman tools: `postman_get_collection`, `postman_sync_snapshot`, `postman_diff_snapshot`, `postman_search_requests`
- `src/postman/` — Postman API client, diff, snapshot write
- `scripts/check-postman.mjs` — CI-проверка postman helpers

### Fixed

- `npm test` больше не падает на отсутствующем `check-postman.mjs`
- Документация синхронизирована с кодом (README, ECOSYSTEM, EXAMPLES `#postman-tools`)

## [1.2.0] - 2026-06-22

### Added

- Auto typed tools (`api_*`) для всех HTTP-методов `api.samotpravil.ru`, не покрытых ручными tools (~42)
- MCP Prompts: `integration_overview`, `send_transactional`, `stop_list_workflow`, `check_delivery`
- HTTP transport: `npx samotpravil-mcp --http [--port 3000]` (Streamable HTTP, stateless)
- `npm run export-openapi` → `data/openapi.yaml`
- SwaggerHub: upload/check scripts, [docs/SWAGGERHUB.md](./docs/SWAGGERHUB.md) (публично: `mailganer/samotpravil-smtp-api@1.0.0`)
- `server.json` для MCP Registry
- Уникальные endpoint slugs при коллизиях (`smtp_send` сохранён)
- [docs/ROADMAP_v1.2.md](./docs/ROADMAP_v1.2.md)

## [1.1.0] - 2026-06-21

### Added

- [docs/official/](./docs/official/) — HTML-блок, snippet для get-access, черновик блога, запрос для docs-команды
- Milestone v1.1.0: docs tools, typed API, resources, offline snapshot, safety, contributor docs

### Note

Публикация на documentation.samotpravil.ru требует правки Postman-коллекции командой Samotpravil — см. [docs/official/README.md](./docs/official/README.md).

## [1.0.5] - 2026-06-21

### Added

- [CONTRIBUTING.md](./CONTRIBUTING.md) — onboarding для контрибьюторов
- Расширенный [docs/EXAMPLES.md](./docs/EXAMPLES.md) — 3 сценария (send, stop-list, status)
- PR template и issue template «New API endpoint»

## [1.0.4] - 2026-06-21

### Added

- MCP Resources: `samotpravil://overview`, `endpoints`, `endpoint/{slug}`, `errors`, `integration`

## [1.0.3] - 2026-06-21

### Added

- 9 typed API tools: `send_email`, `send_mail_v2`, `get_delivery_status`, `get_package_status`, `search_stop_list`, `add_stop_list_email`, `remove_stop_list_email`, `validate_email`, `list_allowed_domains`
- Safety env: `SAMOTPRAVIL_READ_ONLY`, `SAMOTPRAVIL_ALLOW_SEND`
- `dry_run` on typed tools and `api_request`

## [1.0.2] - 2026-06-21

### Added

- Bundled Postman snapshot: `data/collection.snapshot.json`
- `npm run sync-docs` — обновление snapshot с documentation.samotpravil.ru
- Offline fallback: live → snapshot (`SAMOTPRAVIL_DOCS_MODE=auto|live|snapshot`)
- `get_overview`: источник документации, publishDate, syncedAt

## [1.0.1] - 2026-06-21

### Added

- Публикация в npm: установка через `npx -y samotpravil-mcp`
- [docs/EXAMPLES.md](./docs/EXAMPLES.md) — конфиги для Cursor, Claude Desktop, VS Code
- GitHub Actions workflow `publish.yml` — publish on tag `v*`
- README: npx как основной способ установки

### Changed

- `setup.sh` помечен как путь для local dev / fork

## [1.0.0] - 2026-06-21

- Initial release: docs tools + `api_request`
- Postman collection loader from documentation.samotpravil.ru

[1.1.0]: https://github.com/dkanster/samotpravil-api-mcp/compare/v1.0.5...v1.1.0
[1.0.5]: https://github.com/dkanster/samotpravil-api-mcp/compare/v1.0.4...v1.0.5
[1.0.4]: https://github.com/dkanster/samotpravil-api-mcp/compare/v1.0.3...v1.0.4
[1.0.3]: https://github.com/dkanster/samotpravil-api-mcp/compare/v1.0.2...v1.0.3
[1.0.2]: https://github.com/dkanster/samotpravil-api-mcp/compare/v1.0.1...v1.0.2
[1.0.1]: https://github.com/dkanster/samotpravil-api-mcp/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/dkanster/samotpravil-api-mcp/releases/tag/v1.0.0
