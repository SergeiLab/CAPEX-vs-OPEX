# CAPEX/OPEX Classification

Автоматическая классификация IT-задач по типу затрат (CAPEX vs OPEX) с использованием ML.

## Описание задачи

Модель определяет тип затрат для IT-задач:
- **CAPEX** (Capital Expenditure) - капитальные затраты на развитие
- **OPEX** (Operational Expenditure) - операционные затраты на поддержку

## Структура проекта

```
.
├── train.ipynb                    # Обучение модели
├── predict.ipynb                  # Генерация предсказаний
├── requirements.txt               # Зависимости
├── capex_opex_model.pkl          # Обученная модель (после запуска train.ipynb)
└── README.md
```

## Установка

```bash
pip install -r requirements.txt
```

## Использование

### 1. Обучение модели

```python
# Положите capex_opex_train.csv в текущую директорию
# Запустите train.ipynb
```

**Результат:** файл `capex_opex_model.pkl` с обученной моделью

### 2. Предсказание

```python
# Положите capex_opex_public_test.csv в текущую директорию
# Запустите predict.ipynb
```

**Результат:** файл `sample_submission_seed42.csv` с предсказаниями

## Google Colab

```python
# Установка
!pip install -q pandas numpy scikit-learn xgboost scipy

# Загрузка данных
from google.colab import files
files.upload()  # capex_opex_train.csv

# Обучение - вставьте код из train.ipynb

# Загрузка теста
files.upload()  # capex_opex_public_test.csv

# Предсказание - вставьте код из predict.ipynb

# Скачать результат
files.download('sample_submission_seed42.csv')
```

## Технические детали

**Признаки:**
- TF-IDF векторизация текста (task_subject + work_description)
- Кодирование категорий (task_type, task_status)
- Логарифм часов работы

**Модель:** XGBoost Classifier

**Метрика:** F1-score (macro average)

## Формат данных

### Входные данные (train)
| Поле | Тип | Описание |
|------|-----|----------|
| task_subject | text | Заголовок задачи |
| work_description | text | Описание работы |
| hours | float | Человеко-часы |
| task_type | category | Тип задачи |
| task_status | category | Статус |
| target | category | CAPEX или OPEX |

### Выходные данные
CSV файл с одним столбцом `target` (CAPEX/OPEX для каждой строки теста)

## Производительность

F1-score (macro): ~0.75-0.85 на валидации

## Зависимости

- pandas 2.0.3
- numpy 1.24.3
- scikit-learn 1.3.0
- xgboost 2.0.3
- scipy 1.11.1

## Лицензия

MIT