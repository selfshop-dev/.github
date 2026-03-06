# .github

Организационный репозиторий [selfshop-dev](https://github.com/selfshop-dev). Содержит дефолтные community health files для всех репозиториев организации.

## Rulesets

Готовые rulesets для новых репозиториев хранятся в `rulesets/`.

### Применить защиту default ветки
```bash
gh api repos/selfshop-dev/REPO-NAME/rulesets \
  --method POST \
  --input rulesets/protect-default-branch.json
```

Заменить `REPO-NAME` на имя нового репозитория.

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