# beer4rec

**Персональные рекомендации пива по истории оценок BeerAdvocate.**

Проектируем блок «Рекомендуем вам»: система ранжирует новые для пользователя сорта
по его предпочтениям. Основная гипотеза — превзойти неперсональный TopPop по NDCG@10
на офлайн-проверке по времени. Первый вариант модели — матричная факторизация
с обучением ALS на явных оценках `overall`.
Аспектные оценки вкуса, аромата, внешнего вида и ощущения во рту — направление улучшения.

Учебный проект курса ML System Design. Текущий этап — **ДЗ1: настройки репозитория
и проектирование MVP**; модели и инференс пока не реализованы.
Требования, данные, протокол оценки и схемы описаны в
[дизайн-документе](docs/ml_system_design_doc.md).

## Данные

[CSV-зеркало BeerAdvocate](https://www.kaggle.com/datasets/rdoume/beerreviews):
1 586 614 отзывов, 33 387 пользователей и 66 055 сортов. В файле есть общие и аспектные
оценки, даты, стиль, крепость и пивоварня; текстов отзывов нет.

Скачать и распаковать файл в `data/raw/beer_reviews.csv`.
Данные хранятся локально и исключены из Git. Загрузчик и EDA будут добавлены
на следующем этапе.

## Окружение и проверки

Нужны [uv](https://docs.astral.sh/uv/getting-started/installation/) и Python 3.12.
Зависимости зафиксированы в `uv.lock`; отсутствующий Python uv может установить автоматически.

```bash
uv sync --locked
uv run pre-commit install
uv run ruff check .
uv run ruff format --check .
uv run pre-commit run --all-files
```

pre-commit проверяет Ruff, YAML, TOML, следы конфликтов, размер файлов и пробелы.
`--all-files` охватывает файлы, известные Git; новый файл до добавления в индекс
можно проверить через `uv run pre-commit run --files path/to/file`.
Python-модулей на этапе ДЗ1 ещё нет, поэтому проверка Python-кода может быть пропущена.

Применить исправления и форматирование:

```bash
uv run ruff check --fix .
uv run ruff format .
```

## Структура

```text
beer4rec/
├── docs/ml_system_design_doc.md
├── .gitignore
├── .pre-commit-config.yaml
├── .python-version
├── pyproject.toml
├── uv.lock
└── README.md
```
