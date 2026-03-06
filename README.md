# .github

Организационный репозиторий [selfshop-dev](https://github.com/selfshop-dev). Содержит дефолтные community health files для всех репозиториев организации.

## Rulesets

Готовые rulesets для новых репозиториев хранятся в `rulesets/`.

### protect-default-branch

Защищает ветки `main` и `dev`: требует PR, линейную историю, прохождение CI-чеков и блокирует force push.

**Применить к новому репозиторию:**
```bash
gh api repos/selfshop-dev/REPO-NAME/rulesets \
  --method POST \
  --input <(curl -s https://raw.githubusercontent.com/selfshop-dev/.github/main/rulesets/protect-default-branch.json)
```

Заменить `REPO-NAME` на имя репозитория.

### Что наследуется автоматически

| Файл | Назначение |
|---|---|
| `PULL_REQUEST_TEMPLATE.md` | Шаблон описания PR |
| `SECURITY.md` | Политика безопасности |
| `ISSUE_TEMPLATE/` | Шаблоны issues |

### Issue Templates

| Шаблон | Когда использовать |
|---|---|
| 🐛 Bug report | Ошибка или некорректное поведение |
| ✨ Feature request | Новая функциональность или улучшение |
| 💬 Question | Вопрос по использованию или архитектуре |

Для уязвимостей — только [Private Vulnerability Reporting](../../security/advisories/new), не через публичный issue.

## Лицензия

[MIT](LICENSE) © 2026 selfshop-dev