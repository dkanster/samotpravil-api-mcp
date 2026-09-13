# Публикация в npm

Interim-пакет: **`samotpravil-mcp`** (аккаунт maintainer).  
Будущий scoped-пакет: **`@samotpravil/mcp`** — после создания npm org.

## Первый publish

1. Создайте [npm access token](https://www.npmjs.com/settings/~youraccount/tokens) (type: **Automation** для CI, или **Publish** для ручного).
2. В GitHub repo: **Settings → Secrets → Actions → New repository secret**
   - Name: `NPM_TOKEN`
   - Value: npm token
3. Убедитесь, что версия в `package.json` обновлена и `CHANGELOG.md` содержит секцию версии.
4. Проверка перед tag:

```bash
npm run release-prepare
```

Или по шагам:

```bash
npm run sync-versions
npm run pre-publish-check
npm test
```

5. Создайте и запушьте tag:

```bash
git tag v1.0.1
git push origin v1.0.1
```

Workflow [.github/workflows/publish.yml](../.github/workflows/publish.yml) выполнит `npm test` и `npm publish`.

## Ручной publish (локально)

```bash
npm login
npm test
npm publish --access public
```

## Проверка после publish

```bash
npx -y samotpravil-mcp@latest --help 2>&1 | head -1
# или запуск через MCP config из docs/EXAMPLES.md
```

---

## MCP Registry

Метаданные сервера: [`server.json`](../server.json).  
`package.json` → `mcpName` **должен совпадать** с `server.json` → `name`.

### CI (рекомендуется)

При push tag `v*` workflow [.github/workflows/publish.yml](../.github/workflows/publish.yml) публикует в npm; после успешного завершения [.github/workflows/mcp-registry.yml](../.github/workflows/mcp-registry.yml) публикует в [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io) (через `workflow_run`, с ожиданием появления пакета на npm).

Ручной запуск registry:

```bash
gh workflow run "Publish MCP Registry" --repo dkanster/samotpravil-api-mcp
```

### Smithery

Файл [`smithery.yaml`](../smithery.yaml) в корне — конфигурация install wizard на [smithery.ai](https://smithery.ai). После merge в main подключите репозиторий в Smithery Dashboard (Build from GitHub).

### Локально

```bash
brew install mcp-publisher
mcp-publisher validate server.json
mcp-publisher login github
mcp-publisher publish server.json
```

### Проверка

```bash
curl -s "https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.dkanster/samotpravil-api-mcp" | head -c 500
```

---

## Cursor Directory

Каталог сообщества: [cursor.directory](https://cursor.directory). Автодетект MCP из корневого [`.mcp.json`](../.mcp.json) (дубль [`mcp.json`](../mcp.json), манифест [`plugin.json`](../plugin.json)). В листинг не входят swagger-mcp и Postman maintainer.

Первая отправка (нужен вход GitHub или Google):

1. https://cursor.directory/plugins/new
2. URL: `https://github.com/dkanster/samotpravil-api-mcp`

Это не официальный Marketplace Cursor (`cursor.com/marketplace`).

### Как вносить изменения после публикации

Три поверхности. Правка одной **не** обновляет остальные сама.

| Что меняете | Куда | Когда доходит до людей |
|---|---|---|
| Tools, тексты, флаги безопасности | этот репо (`src/`, тесты) + **релиз npm** | `npx -y samotpravil-mcp@latest` после tag `v*` |
| Дефолты Add to Cursor, описание карточки | `.mcp.json`, `mcp.json`, `plugin.json`, README | существующая карточка Directory; у кого уже нажато Add — только повторное добавление |
| Узкий Python MCP команды Mailganer | GitLab `mg/mailganer-mcp` (`samotpravil_mcp/`) | только локальный setup; сюда и в npm **не** едет |

Пуш в `main` без тега **не** обновляет пакет у тех, кто ставит через `npx`. Релиз — секции выше (tag `v*`).

Карточка Directory:

1. Залогиниться на cursor.directory тем же аккаунтом, которым слали карточку.
2. Править **уже существующую** запись или заново просканировать **тот же** URL репо.
3. **Не** жать Submit как новый плагин с тем же репозиторием: появляются дубли (`…-1`), удаление с сайта часто не срабатывает.

У кого уже нажато Add to Cursor, фрагмент в их `mcp.json` сам не обновится. Код MCP может подтянуться через `@latest` после npm-релиза.

Не класть боевой ключ в `.mcp.json` / README: только `${SAMOTPRAVIL_API_KEY}`. По умолчанию `SAMOTPRAVIL_ALLOW_SEND=0` и мутации выключены.
