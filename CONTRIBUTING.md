# Contributing

## Флоу

```
feature-branch → dev → main
```

1. Создайте ветку от `dev`: `feat/my-feature` / `fix/my-fix`
2. Разработайте, соблюдая [Conventional Commits](https://www.conventionalcommits.org/)
3. Убедитесь, что CI зелёный: `make lint && make test`
4. Откройте PR **в ветку `dev`**, заполните шаблон

> PR напрямую в `main` блокируются автоматически.

## Соглашение по коммитам

Проект следует [Conventional Commits](https://www.conventionalcommits.org). Формат: `<type>: <summary>`, где summary — повелительное наклонение, английский язык, без точки в конце.

| Тип | Когда использовать |
|---|---|
| `feat` | Новая функциональность |
| `fix` | Исправление бага |
| `refactor` | Рефакторинг без изменения поведения |
| `perf` | Оптимизация производительности |
| `test` | Добавление или изменение тестов |
| `docs` | Документация |
| `ci` | Изменения CI/CD и GitHub Actions |
| `build` | Сборка, зависимости, инфраструктура проекта |
| `chore` | Рутинные задачи, не попадающие в другие типы |

Тип коммита автоматически определяет метку PR и тип следующего релиза: `feat` → minor, `fix` / `perf` / `refactor` → patch, `feat!` / `fix!` (breaking change) → major.

## Требования к коду

Описаны в [PULL_REQUEST_TEMPLATE.md](https://github.com/selfshop-dev/.github/blob/main/PULL_REQUEST_TEMPLATE.md?plain=1) в разделе Checklist

## Issues

Используйте шаблоны: 🐛 Bug / ✨ Feature / 💬 Question.  
Уязвимости — только через [Private Vulnerability Reporting](../../security/advisories/new).