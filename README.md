# beer4rec

**Персональные рекомендации пива по истории оценок BeerAdvocate.**

Проектируем блок «Рекомендуем вам»: система ранжирует новые для пользователя сорта
по его предпочтениям. Основная гипотеза — превзойти выбранный неперсональный бейзлайн
(TopPop, TopRated или TopPositive) по NDCG@10 на временной проверке по всему доступному каталогу. Первый вариант модели — матричная факторизация
с обучением ALS на явных оценках `overall`.
Аспектные оценки вкуса, аромата, внешнего вида и ощущения во рту — направление улучшения.

Учебный проект курса ML System Design. Текущий этап — **ДЗ2: EDA и план решения**.
См. [ноутбук EDA](notebooks/beeradvocate_eda.ipynb) и [дизайн-документ](docs/ml_system_design_doc.md).
Обучение и инференс запланированы на следующие этапы.

## Данные

[CSV-зеркало BeerAdvocate](https://www.kaggle.com/datasets/rdoume/beerreviews):
1 586 614 отзывов, 33 387 пользователей и 66 055 сортов. В файле есть общие и аспектные
оценки, даты, стиль, крепость и пивоварня; текстов отзывов нет.

Скачать и распаковать файл в `data/raw/beer_reviews.csv`.
Данные хранятся локально и исключены из Git. В ноутбуке уже сохранены таблицы и графики.
Чтобы пересчитать их, запустите Jupyter из корня проекта:

```bash
uv run --no-project --python 3.12 --with notebook --with matplotlib==3.10.3 \
  jupyter notebook notebooks/beeradvocate_eda.ipynb
```

В Jupyter выберите **Restart Kernel and Run All Cells**. Для другого файла измените
`CSV_PATH` в первой ячейке. Ноутбук выводит только агрегаты; исходные отзывы не сохраняет.

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

Применить исправления и форматирование:

```bash
uv run ruff check --fix .
uv run ruff format .
```

## Структура

```text
beer4rec/
├── docs/ml_system_design_doc.md
├── notebooks/beeradvocate_eda.ipynb
├── .gitignore
├── .pre-commit-config.yaml
├── .python-version
├── pyproject.toml
├── uv.lock
└── README.md
```
