# .github

Организационный репозиторий [selfshop-dev](https://github.com/selfshop-dev). Содержит дефолтные community health files и reusable workflows для всех репозиториев организации.

## Rulesets

Готовые rulesets для новых репозиториев хранятся в [`rulesets/`](rulesets/).

| Ruleset | Ветка | Назначение |
|---|---|---|
| [`protect-main-branch`](rulesets/protect-main-branch.json) | `main` | Полная защита: CI + Security + CodeQL + Trivy |
| [`protect-main-branch-minimal`](rulesets/protect-main-branch-minimal.json) | `main` | Минимальная защита: только PR + линейная история, без CI чеков |
| [`protect-dev-branch`](rulesets/protect-dev-branch.json) | `dev` | Базовая защита: только Lint + Test |

### Сравнение правил

| Правило | `protect-main-branch` | `protect-main-branch-minimal` | `protect-dev-branch` |
|---|---|---|---|
| Запрет удаления | ✅ | ✅ | ✅ |
| Запрет force push | ✅ | ✅ | ✅ |
| Линейная история | ✅ | ✅ | ✅ |
| Обязательный PR | ✅ | ✅ | ✅ |
| Thread resolution | ✅ | ✅ | ❌ |
| Lint + Test | ✅ | ❌ | ✅ |
| Block non-dev to main | ✅ | ❌ | ❌ |
| CodeQL | ✅ | ❌ | ❌ |
| Trivy | ✅ | ❌ | ❌ |
| Strict status checks | ✅ | ❌ | ❌ |
| Разрешённые методы merge | squash, rebase | squash, rebase | squash, rebase |

**Применить к новому репозиторию:**

```bash
# ruleset protect-main-branch
gh api repos/selfshop-dev/REPO-NAME/rulesets \
  --method POST \
  --input <(curl -s https://raw.githubusercontent.com/selfshop-dev/.github/main/rulesets/protect-main-branch.json)

# ruleset protect-dev-branch
gh api repos/selfshop-dev/REPO-NAME/rulesets \
  --method POST \
  --input <(curl -s https://raw.githubusercontent.com/selfshop-dev/.github/main/rulesets/protect-dev-branch.json)
```

> Заменить `REPO-NAME` на имя репозитория.

## Reusable Workflows

Все workflows используются через Platform Engineering подход — один источник правды для всей организации.

```yaml
jobs:
  ci:
    uses: selfshop-dev/.github/workflows/ci.yml@v1
    secrets: inherit
```

### Версионирование

Workflows версионируются через Git tags по схеме `vMAJOR.MINOR.PATCH` с floating major tags.

| Tag | Поведение |
|---|---|
| `@v1` | Floating tag — получает все патчи и минорные обновления автоматически |
| `@v1.2.3` | Точная версия — никогда не меняется |
| `@main` | Нестабильный — не использовать в сервисных репо |

Breaking change в workflow → новый major tag (`v2`). Всё остальное — patch/minor под тем же `v1`.

Floating tags (`v1`, `v1.2`) обновляются автоматически при публикации релиза через [`update-floating-tags.yml`](.github/workflows/update-floating-tags.yml) workflow.

### Доступные workflows

| Workflow | Назначение | Конфиг в репо |
|---|---|---|
| [`ci.yml`](.github/workflows/ci.yml) | Lint + Test с coverage upload | `.golangci.yml` |
| [`codeql.yml`](.github/workflows/codeql.yml) | Статический анализ кода (Go) | `.github/codeql.yml` |
| [`security.yml`](.github/workflows/security.yml) | Trivy filesystem scan | — |
| [`block-non-dev-to-main.yml`](.github/workflows/block-non-dev-to-main.yml) | Блокировка PR не из `dev` в `main` | — |
| [`labeler.yml`](.github/workflows/labeler.yml) | Автоматическая расстановка меток PR и issues | `.github/labeler.yml` |
| [`release-drafter.yml`](.github/workflows/release-drafter.yml) | Автогенерация release notes | `.github/release-drafter.yml` |

> Конфигурационные файлы уникальны для каждого репозитория и не наследуются.

### Пример подключения в сервисном репо

```yaml
name: CI and Security

on:
  pull_request:
    branches: [ main, dev ]

jobs:
  ci:
    uses: selfshop-dev/.github/workflows/ci.yml@v1
    secrets: inherit

  security:
    uses: selfshop-dev/.github/workflows/security.yml@v1
```

## Что наследуется автоматически

| Файл | Назначение |
|---|---|
| [`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md) | Шаблон описания PR |
| [`SECURITY.md`](SECURITY.md) | Политика безопасности |
| [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) | Кодекс поведения |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Руководство для контрибьюторов |
| [`ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE/) | Шаблоны issues |

## Issue Templates

| Шаблон | Когда использовать |
|---|---|
| [`🐛 Bug report`](.github/ISSUE_TEMPLATE/bug_report.yml) | Ошибка или некорректное поведение |
| [`✨ Feature request`](.github/ISSUE_TEMPLATE/feature_request.yml) | Новая функциональность или улучшение |
| [`💬 Question`](.github/ISSUE_TEMPLATE/question.yml) | Вопрос по использованию или архитектуре |

Для уязвимостей — только **[Private Vulnerability Reporting](../../security/advisories/new)**, не через публичный issue.

## Лицензия

[MIT](LICENSE) © 2026-present [selfshop-dev](https://github.com/selfshop-dev)