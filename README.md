# .github

Организационный репозиторий [selfshop-dev](https://github.com/selfshop-dev). Содержит дефолтные community health files и reusable workflows для всех репозиториев организации.

## Rulesets

Готовые rulesets для новых репозиториев хранятся в `rulesets/`.

| Ruleset | Ветка | Назначение |
|---|---|---|
| `protect-main-branch` | `main` | Полная защита: CI + Security + CodeQL + Trivy |
| `protect-main-branch-minimal` | `main` | Минимальная защита: только PR + линейная история, без CI чеков |
| `protect-dev-branch` | `dev` | Базовая защита: только Lint + Test |

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
gh api repos/selfshop-dev/REPO-NAME/rulesets \
  --method POST \
  --input <(curl -s https://raw.githubusercontent.com/selfshop-dev/.github/main/rulesets/protect-main-branch.json)

gh api repos/selfshop-dev/REPO-NAME/rulesets \
  --method POST \
  --input <(curl -s https://raw.githubusercontent.com/selfshop-dev/.github/main/rulesets/protect-dev-branch.json)
```

> Заменить `REPO-NAME` на имя репозитория. Применять только нужные rulesets — не все три сразу.

## Reusable Workflows

Все workflows используются через Platform Engineering подход — один источник правды для всей организации.

```yaml
jobs:
  ci:
    uses: selfshop-dev/.github/workflows/ci.yml@v1
```

### Версионирование

Workflows версионируются через Git tags по схеме `vMAJOR.MINOR.PATCH` с floating major tags.

| Tag | Поведение |
|---|---|
| `@v1` | Floating tag — получает все патчи и минорные обновления автоматически |
| `@v1.2.3` | Точная версия — никогда не меняется |
| `@main` | Нестабильный — не использовать в сервисных репо |

Breaking change в workflow → новый major tag (`v2`). Всё остальное — patch/minor под тем же `v1`.

Floating tags (`v1`, `v1.2`) обновляются автоматически при публикации релиза через `update-floating-tags` workflow.

### Доступные workflows

| Workflow | Назначение | Конфиг в репо |
|---|---|---|
| `ci.yml` | Lint + Test с coverage upload | `.golangci.yml` |
| `codeql.yml` | Статический анализ кода (Go) | `.github/codeql.yml` |
| `security.yml` | Trivy filesystem scan | — |
| `block-non-dev-to-main.yml` | Блокировка PR не из `dev` в `main` | — |
| `labeler.yml` | Автоматическая расстановка меток PR и issues | `.github/labeler.yml` |
| `release-drafter.yml` | Автогенерация release notes | `.github/release-drafter.yml` |

> Конфигурационные файлы уникальны для каждого репозитория и не наследуются.

### Пример подключения в сервисном репо

```yaml
name: CI

on:
  pull_request:
    branches: [ main, dev ]
  push:
    branches: [ main ]

jobs:
  ci:
    uses: selfshop-dev/.github/workflows/ci.yml@v1

  codeql:
    uses: selfshop-dev/.github/workflows/codeql.yml@v1

  security:
    uses: selfshop-dev/.github/workflows/security.yml@v1

  block:
    uses: selfshop-dev/.github/workflows/block-non-dev-to-main.yml@v1
```

## Что наследуется автоматически

| Файл | Назначение |
|---|---|
| `PULL_REQUEST_TEMPLATE.md` | Шаблон описания PR |
| `SECURITY.md` | Политика безопасности |
| `CODE_OF_CONDUCT.md` | Кодекс поведения |
| `CONTRIBUTING.md` | Руководство для контрибьюторов |
| `ISSUE_TEMPLATE/` | Шаблоны issues |

## Issue Templates

| Шаблон | Когда использовать |
|---|---|
| 🐛 Bug report | Ошибка или некорректное поведение |
| ✨ Feature request | Новая функциональность или улучшение |
| 💬 Question | Вопрос по использованию или архитектуре |

Для уязвимостей — только [Private Vulnerability Reporting](../../security/advisories/new), не через публичный issue.

## Лицензия

[MIT](LICENSE) © 2026-present [selfshop-dev](https://github.com/selfshop-dev)